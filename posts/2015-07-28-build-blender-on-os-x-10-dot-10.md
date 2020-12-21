layout: post
title: "Build Blender on OS X 10.10"
date: 2015-07-28 11:47:43 +0800
comments: true
Category: Blender
Tags: Build
Has_Math: True


__Notes: This description here is still valid in OS X 10.11 (El Capitan) / Xcode 7.0 / CMake 3.3. For macOS Sierra, refer to [Command Line Build Blender](http://rd.coding-addict.com/command-line-build-blender.html) __

Currently, the Blender source has been to 2.75.3

The Xcode has been to Xcode 6.4 (Apple has release Xcode 7 beta)

CMake is 3.3

<!--More-->

##Building With Scons

The simpler way to build is using Scons, it is a set of python scripts that will act like CMake. Under OS X, we first create `user-config.py` under the blender root source:

	CC = '../lib/darwin-9.x.universal/clang-omp-3.5/bin/clang'
	CXX = '../lib/darwin-9.x.universal/clang-omp-3.5/bin/clang++'

This is due to the Apple LLVM is lacking of the OpenMP support, we use the Clang in the Blender lib, which dose support it.

We can use this command to check the CPU units on our machine:

	sysctl hw.logicalcpu

And to configure the parameter `-jx` (x=1..8)

Then run this command from the blender source root, I use 4 units to compile:

	python scons/scons.py -j4

If the process is successful, the result `blender.app` will be located in ../install/darwin directory


##Building With Xcode

If we want to TRACE CODE after build, the Xcode is a good candidate.

Following the `Building With Xcode` section in the [official Blender build guide](http://wiki.blender.org/index.php/Dev:Doc/Building_Blender/Mac).

Like [before](http://rd.coding-addict.com/blog/2014/04/28/build-blender-on-os-x-10-dot-9-2/), we have to tell CMake where is the `numpy`, just set `PYTHON_NUMPY_PATH` to

"/Library/Frameworks/Python.framework/Versions/3.4/lib/python3.4/site-packages"

_ps_: If you have installed Python via __conda__, then set`PYTHON_NUMPY_PATH` to where the __conda__ has installed the packages, for example:

"/Users/tom/Library/miniconda3/pkgs"

![](/images/cmake setup numpy path.png)

Setup the `CMAKE_BUILD_TYPE` from `Release` to `Debug`.

![](/images/build-blender-debug.png)

Here, we have 2 choices

***Choice 1 : Disable `Cycles` part:***

![](/images/build-blender-without-cycles.png)

***Choice 2 : Enable `Cycles` part: (blender default)***

This is default settings, while enable `Debug` will cause [link error](https://stackoverflow.com/questions/31178003/how-to-build-a-blender-build-in-xcode-5/33130336#33130336).

The bug is caused by CMake __NOT__ converting correctly the `-gline-tables-only` option in cmake file for `bf_intern_cycles` target, currently the workaround is to add the option manually after the Xcode project has been created:


![](/images/buile-blender-manually-add-other-c-flags.png)

After that (either ***choice 1 or choice 2*** ), hit `generate`, we will have Blender.xcodeproj in the target directory, the first time open it will see:

![](/images/create scheme automatically.png)

Select `Automatically Create Schemes`, then we select the `install` scheme in Xcode.

Since in Xcode6 we need upgrade the configuation, first `clean` your project and you will get this warning:

![](/images/build-blender-conform-to-xcode.png)

finally, of course, perform the change, then we can start build with Xcode.


Note:

Reference:  [http://blender.stackexchange.com/questions/8710/how-to-compile-blender-from-source-on-osx](http://blender.stackexchange.com/questions/8710/how-to-compile-blender-from-source-on-osx)
