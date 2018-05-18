Revised RHESSys - The Regional Hydro-Ecologic Simulation System
===============================================================

This repository is forked from [the official repository of RHESSys](https://github.com/RHESSys/RHESSys).
The aim is to compile RHESSys on Windows OS under MinGW environment.

# 1. Prerequisite 

+ CMake2.8+
+ Windows 7+
+ CLion and mingw64 4.8+
+ GRASS GIS 6.4.4 compiled with MinGW, please refers to the [tutorial](https://trac.osgeo.org/grass/wiki/CompileOnWindows)


# 2. Compile procedure

I highly recommend the user-friendly and cross-platform IDE [CLion](https://www.jetbrains.com/clion/).

CLion use CMake to manage projects, so the meanly difference between the repository with the official
one is that I have added a `CMakeLists.txt` to support `CMake` building.

Using CLion is quite easy and intuitive. Just open the RHESSys source code path from `File -> Open...`. Then CLion will automatically load the project by CMakeLists.txt existed in RHESSys directory. 

Now, you can build RHESSys model by typing `Ctrl+F9` or clicking the build button.

# 3. Update with the latest RHESSys source (For administrator only)

+ Add the official RHESSys repository as `upstream` (only do once at the very beginning of this repository)

  ```bash
  cd RHESSys
  git remote add upstream git@github.com:RHESSys/RHESSys.git
  ```

+ Do any modification to meet our specific needs.

+ If the RHESSys source is updated we should pull the latest code from `upstream` and merge it.

  ```shell
  git fetch upstream master
  git checkout master
  git merge upstream/master
  ```

+ Handle conflict manually if needed.

# 4. Changelog

+ 05/18/2018: Update the source code to commit `#a2f5b746a1770a367377159541b509830fd26276` of master branch.