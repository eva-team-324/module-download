Module Download Guide
=====================

A simple guide on how to download and transfer python modules.

# Keynotes

When downloading modules, the following factors must be considered:

## Python version

The module you download must match the version of Python environment it's going to be executed in. Must be specified in <MAJOR.MINOR> format, e.g. 3.8, 3.9, 3.11, etc.

I sincerely hope you know your Python version, but if you have to make sure:
```sh
<your-python-interpreter> --version
```

## Destination platform

Some modules are not platform agnostic and need to be compiled for specific operating systems and underlying CPU architectures. For example, platforms `win32` and `win_am64`, although Windows, are two different platforms. Therefore, their modules are not interchangeable!

Run the following command on the destination machine in order to determine its platform:
```sh
<your-python-interpreter> -c "from distutils.util import get_platform; print(get_platform().replace('-','_'))"
```

- Source distribution or Wheel

## Source distribution vs Wheel

In short, source distribution (or `sdist`) is the source code of the module. It comes in a `.tar.gz` file and is not dependent on any specific platform or Python version. Wheel, on the other hand, is a source distribution which has been precompiled for specific platform and Python version. Wheel comes in a `.whl` file (which is actually a regular `.zip` file), is lighter in terms of weight and does not need to be build or compiled. All modules which come in `sdist` format will have to be compiled before they are installed. Therefore, if your destination platform is known beforehand, it's much easier to just download the wheel and be done with it.

# Procedure

TODO
