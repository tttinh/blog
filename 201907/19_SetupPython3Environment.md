# Install Python 3 and Set Up a Programming Environment on Ubuntu 18.04

[Home](../README.md)

In this article, we will install the latest version of Python3 on our Ubuntu 18.04 system and then set up a virtual programming environment where we can write and execute our Python application programs.

## Install Python 3

### Update and Upgrade

First update and upgrade your system to ensure that your shipped version of Python 3 is up-to-date.

```bash
sudo apt update
sudo apt -y upgrade
```

Confirm installation if prompted to do so.

### Check Version of Python

Check which version of Python 3 is installed by typing:

```bash
$ python3 -V
```

You’ll receive output similar to the following, depending on when you have updated your system.

```
Python 3.6.8
```

Since we already have Python installed on our system, as verified in the previous section, we only need to upgrade it to the latest version as follows:

```bash
$ sudo apt-get upgrade python3
```

In case you did not have Python installed in the first place, you can install it as sudo through the following command after running apt-get update:

```bash
$ sudo apt-get install python3
```

### Install Python3 From Source

In case you want to install the latest Python version that your system does not provide. Following these guidelines to install it from source code.

First, prepare your system by install prerequisite packages to build Python from source.

```bash
$ sudo apt install build-essential libssl-dev libffi-dev python3-dev
```

Second, go to this [website](https://www.python.org/downloads/source/) to check for the latest Python version, then download its `.tgz` source file by the following command:

```bash
wget https://www.python.org/ftp/python/3.7.4/Python-3.7.4.tgz
```

When the file download is complete, run the following command in order to extract the resources:

```bash
tar xvzf Python-3.7.4.tgz
```

Third, create a folder in order to install Python cause we don't want to mess up our system. In this case, I will create a folder named `opt` at my home directory.

```bash
$ mkdir ~/opt
```

Change the directory to `Python-3.7.4`, or to whatever download version you have extracted:

```bash
$ cd Python-3.7.4
```

Run the following command to run the configuration script:

```bash
$ ./configure --enable-optimizations --prefix=/home/$USER/opt/py374
```

Now is the time to install Python.

```bash
$ make install
```

If you can not run the `make` command, you might need to install `make` through the following command:

```bash
$ sudo apt-get make
```

#### Errors that might be encountered during installation

**Error 1**

When you run the `sudo make install` command, you might encounter the following error:

```
...
zipimport.ZipImportError: can't decompress data; zlib not available
Makefile:1132: recipe for target 'install' failed
make: *** [install] Error 1
```

This would mean that a package named `zlib1g-dev` is missing from your system as you might have never needed it before. Run the following command as sudo in order to install the missing `zlib1g-dev` package:

```bash
$ sudo apt install zlib1g-dev
```

Then run the following command in order to complete the Python installation:

```bash
$ sudo make install
```

**Error 2**

When might also get the following error when you run the `sudo make install` command:

```
...
ModuleNotFoundError: No module named '_ctypes'
Makefile:1080: recipe for target 'install' failed
make: *** [install] Error 1
```

This would mean that a package named `libffi-dev` is missing from your system as you might have never needed it before. Run the following command as sudo in order to install the missing `libffi-dev` package:

```bash
$ sudo apt-get install libffi-dev
```

Then run the following command in order to complete the Python installation:

```bash
$ sudo make install
```

A successful install maybe like this:

```
...
Collecting setuptools
Collecting pip
Installing collected packages: setuptools, pip
Successfully installed pip-19.0.3 setuptools-40.8.0
```

* * *

## Setup Virtual Programming Environment for Python3

First, let us get familiar with what is a Virtual Programming Environment for Python projects. You can assume it as an isolated space on your system where you can create Python projects having their own set of dependencies that do not affect anything outside of the project. When you are inside this environment, you can make use of `python` and `pip` commands directly instead of using `pip3` and `python3` commands. However, outside of this environment, you will have to use the `pip3` and `python3` commands to develop and run your applications.

If you followed the **Install Python3 From Source** step above, you can skip first three following steps. Here is the procedure for you to create and activate a new virtual programming environment for Python:

### Install the Prerequisites

```bash
$ sudo apt install build-essential libssl-dev libffi-dev python3-dev
```

### Install pip

To manage software packages for Python, install pip, a tool that will install and manage libraries or modules to use in your projects.

```bash
$ sudo apt install -y python3-pip
```

Python packages can be installed by typing:

```bash
pip3 install [package_name]
```

Here, `package_name` can refer to any Python package or library, such as Django for web development or NumPy for scientific computing. So if you would like to install NumPy, you can do so with the command `pip3 install numpy`.

### Install venv

Virtual environments enable you to have an isolated space on your server for Python projects. We’ll use `venv`, part of the standard Python 3 library, which we can install by typing:

```bash
$ sudo apt install -y python3-venv
```

### Create a Virtual Environment

Here, we’ll call our new environment `my_env`, but you can call yours whatever you want.

```bash
$ python3.7 -m venv my_env
```

Or if you followed the **Install Python3 From Source** step above:

```bash
$ /home/$USER/opt/py374/bin/python3.7 -m venv my_env
```

### Activate Virtual Environment

Activate the environment using the command below, where `my_env` is the name of your programming environment.

```bash
$ source my_env/bin/activate
```

Your command prompt will now be prefixed with the name of your environment:

### Test Virtual Environment

Open the Python interpreter:

```bash
$ python
```

Note that within the Python 3 virtual environment, you can use the command **python** instead of **python3**, and **pip** instead of **pip3**.

You’ll know you’re in the interpreter when you receive the following output:

```bash
Python 3.7.4 (default, Jul 24 2019, 01:24:49)
[GCC 7.4.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>>
```

Now, use the `print()` function to create the traditional Hello, World program:

```bash
>>> print("Hello, World!")
Hello, World!
```

### Deactivate Virtual Environment

Quit the Python interpreter:

```bash
>>> quit()
```

Then exit the virtual environment:

```bash
$ deactivate
```
