layout: post
title: "Build Blender On OS X 10.9.2"
date: 2014-04-28 17:31:52 +0800
comments: true
Category: Blender
Tags: Build
Has_Math: True

The Blender has evolved for 20 years, which is very successful, since it provides a community for both 3D artists and developers.

The following is the notes for building Blender 2.7 on Mac OS X 10.9.2 with Xcode 5.1.1.
<!-- more -->
##Build tools checklist:##

* __Install Xcode Command Line Tool.__
* __Install CMake__ From [here](http://www.cmake.org/). I use its GUI to configure the source. The version I installed is 2.8.12.
* __Python Install.__ [GUI installation Python 3.4.1 for Mac.](https://www.python.org/downloads/release/python-341/) I have not tried install by `brew` command.
* __Create build-blender directory__.

##Before source configuration...##

The following steps are the work-arounds for problems after following the [official build guide](http://wiki.blender.org/index.php/Dev:Doc/Building_Blender/Mac). Since the guide is somewhat outdated for the related development tools.

* __Get the source.__ Following get source part of the [official build guide](http://wiki.blender.org/index.php/Dev:Doc/Building_Blender/Mac). Dont forget to get the `lib` sources, too.
* __Check pip.__ If you have python 3.4.1 installed, the `pip` command is changed to `pip3`, it is built-in python since 3.4.1.
* __Install numpy.__ The blender uses a lot of numerical operations, __numpy__ is considerd as the default dependancy for Blender.

		> pip3 install numpy

After the __numpy__ being installed, it should be located in `/Library/Frameworks/Python.framework/Versions/3.4/lib/python3.4/site-packages`. Please double check its path.

##Configure Flags##
In my CMake GUI, search `numpy` and set PYTHON_NUMPY_PATH to `/Library/Frameworks/Python.framework/Versions/3.4/lib/python3.4/site-packages`

![Configure NUMPY path](
http://coding-addict.com/pictures/blogger/numpy problem.png)

And __uncheck__ the OpenMP like this:

![Turn off OpenMP](http://coding-addict.com/pictures/blogger/turn_off_OPENMP.png)

After pressing the `Generate` button, you will see:
![Configure NUMPY path](
http://coding-addict.com/pictures/blogger/cmake generate before build.png)

Currently w/o support `international` and `add on`, we can still build a workable version: Go to `build-blender` directory and open the Blender.xcodeproj with XCode. Change build scheme to `blender.app`:
![Configure NUMPY path](
http://coding-addict.com/pictures/blogger/edit scheme for blender app.png)

[LLVM seems not support OpenMP.](https://gitorious.org/blenderprojects/blender/source/b7f52a26364ff0095ccdd1ca4247426129efb028:blender/CMakeLists.txt)



With the above 2 flags work-around. We finally could build the Blender 2.7.#




