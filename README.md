Module Download Guide
=====================

A simple guide on how to download Python modules and their dependencies in order to use them in disconnected environments.

# Keynotes

When downloading modules, the following factors must be considered:

## Python version

The module you download must match the version of the Python environment it's going to be executed in. Must be specified in <MAJOR.MINOR> format, e.g. 3.8, 3.9, 3.11, etc.

I sincerely hope you know which version of Python you need, but if you have to make sure:
```sh
<your-python-interpreter> --version
```

## Destination platform

Some modules are not platform agnostic and need to be compiled for specific operating systems and underlying CPU architectures. For example, platforms `win32` and `win_amd64`, although both Windows, are two different platforms. Therefore, their modules are not interchangeable!

Run the following command on the destination machine in order to determine its platform:
```sh
<your-python-interpreter> -c "from distutils.util import get_platform; print(get_platform().replace('-','_'))"
```

## Source distribution vs Wheel

In short, source distribution (or `sdist`) is the source code of the module. It comes in a `.tar.gz` file and is not dependent on any specific platform or Python version. Wheel, on the other hand, is a source distribution which has been precompiled for specific platform and Python version. Wheel comes in a `.whl` file (which is actually a regular `.zip` file), is lighter in terms of weight and does not need to be built or compiled. All modules which come in `sdist` format will have to be compiled to wheels before they can be installed. Therefore, if your destination platform is known beforehand, it's much simpler to just download the wheel and be done with it.

# Procedure

1. Have Python and Pip installed. Use the Python version which matches the Python version of your modules, just to avoid any issues.

2. Prepare a `requirements.txt` file with the modules that you want to download. Include a specific version if need be, or omit the version in order to download the latest one available. An example `requirements.txt` file is included with this repository.

3. Create a destination directory for your modules. 

4. Download the modules. Pip comes with a `download` subcommand which we will leverage. Consider the following command:
   ```sh
   <your-python-interpreter> -m pip download -r requirements.txt -d deps --python-version 3.8 --platform win32 --only-binary=:all:

   # Collecting smbprotocol==1.10.1pycparser
   #   Downloading smbprotocol-1.10.1-py3-none-any.whl (125 kB)
   #      ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 125.6/125.6 kB 834.3 kB/s eta 0:00:00
   # Collecting pyspnego
   #   Downloading pyspnego-0.9.0-cp38-cp38-win32.whl (230 kB)
   #      ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 230.1/230.1 kB 2.7 MB/s eta 0:00:00
   # Collecting cryptography>=2.0
   #   Downloading cryptography-40.0.2-cp36-abi3-win32.whl (2.2 MB)
   #      ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 2.2/2.2 MB 12.4 MB/s eta 0:00:00
   # Collecting cffi>=1.12
   #   Downloading cffi-1.15.1-cp38-cp38-win32.whl (170 kB)
   #      ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 170.2/170.2 kB 19.9 MB/s eta 0:00:00
   # Collecting pycparser
   #   Using cached pycparser-2.21-py2.py3-none-any.whl (118 kB)
   # Saved ./deps/smbprotocol-1.10.1-py3-none-any.whl
   # Saved ./deps/cryptography-40.0.2-cp36-abi3-win32.whl
   # Saved ./deps/pyspnego-0.9.0-cp38-cp38-win32.whl
   # Saved ./deps/cffi-1.15.1-cp38-cp38-win32.whl
   # Saved ./deps/pycparser-2.21-py2.py3-none-any.whl
   # Successfully downloaded smbprotocol cryptography pyspnego cffi pycparser
   ```

   Note that although `smbprotocol` is the only module in the `requirements.txt`, Pip has also downloaded the relevant depencencies. Let's break down the command:

   - `-r requirements.txt` tells pip to download modules specified in the `requirements.txt` file
   - `-d deps` specifies the destination folder called `deps`
   - `--python-version 3.8` specifies the target Python version
   - `--platform win32` specifies the `win32` platform
   - `--only-binary=:all:` tells pip to only download wheels. In case wheel is not present, Pip will try to download and compile an `sdist` to a wheel.

   Since we specified the platform, Pip forces us to use the `--only-binary=:all:` flag as source distributions are not tied to a platform. In case we only want to download the source distributions, use the `--no-binary=:all:` flag:
   ```sh
   <your-python-interpreter> -m pip download -r requirements.txt -d deps/ --no-binary=:all:

   # Collecting smbprotocol==1.10.1 (from -r requirements.txt (line 1))
   #   Downloading smbprotocol-1.10.1.tar.gz (121 kB)
   #      ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 121.2/121.2 kB 834.9 kB/s eta 0:00:00
   #   Installing build dependencies ... done
   #   Getting requirements to build wheel ... done
   #   Installing backend dependencies ... done
   #   Preparing metadata (pyproject.toml) ... done
   # Collecting cryptography>=2.0 (from smbprotocol==1.10.1->-r requirements.txt (line 1))
   #   Downloading cryptography-40.0.2.tar.gz (625 kB)
   #      ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 625.6/625.6 kB 4.2 MB/s eta 0:00:00
   #   Installing build dependencies ... done
   #   Getting requirements to build wheel ... done
   #   Preparing metadata (pyproject.toml) ... done
   # Collecting pyspnego (from smbprotocol==1.10.1->-r requirements.txt (line 1))
   #   Downloading pyspnego-0.9.0.tar.gz (233 kB)
   #      ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 233.6/233.6 kB 27.8 MB/s eta 0:00:00
   #   Installing build dependencies ... done
   #   Getting requirements to build wheel ... done
   #   Installing backend dependencies ... done
   #   Preparing metadata (pyproject.toml) ... done
   # Collecting cffi>=1.12 (from cryptography>=2.0->smbprotocol==1.10.1->-r requirements.txt (line 1))
   #   Using cached cffi-1.15.1.tar.gz (508 kB)
   #   Preparing metadata (setup.py) ... done
   # Collecting pycparser (from cffi>=1.12->cryptography>=2.0->smbprotocol==1.10.1->-r requirements.txt (line 1))
   #   Using cached pycparser-2.21.tar.gz (170 kB)
   #   Preparing metadata (setup.py) ... done
   # Saved ./deps/smbprotocol-1.10.1.tar.gz
   # Saved ./deps/cryptography-40.0.2.tar.gz
   # Saved ./deps/pyspnego-0.9.0.tar.gz
   # Saved ./deps/cffi-1.15.1.tar.gz
   # Saved ./deps/pycparser-2.21.tar.gz
   # Successfully downloaded smbprotocol cryptography pyspnego cffi pycparser
   ```

   Note that:
   - `--python-version` and `--platform` flags have been omitted as they are not relevant
   - A lot of additional steps have to be performed in order to fully prepare the source distibutions. You will likely need to install additional development packages as well. 
