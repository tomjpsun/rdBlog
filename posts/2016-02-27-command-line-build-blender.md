layout: post
title: "Command Line Build Blender"
date: 2016-02-27 17:06:45 +0800
comments: true
Category: Blender
Tags:Build
Has_Math: True


__ Notes: 2018-01-05 update: The Blender developers has simplified the whold build processes into simple instructions: __
In Blender root source directory, 
	
	$> make update
	
then
	
	$> make
	
and all things are done !!
##

old post

### Introduction###
To build Blender from command line, I need a script to compensate
the [wrong conversion](https://cmake.org/Bug/view.php?id=15806) made by CMake.

<!--More-->

To add 'Other C++ Build Flags' in Xcode project, I use [this script](https://gist.github.com/tomjpsun/6c9e301bbeae000811bd), which uses
[this Python module](https://github.com/kronenthaler/mod-pbxproj).

`note`: The module should run with Python2

###Build Steps###

 	#mkdir build-blender
 	#cd build-blender
 	#cmake ../blender -GXcode
 	#./modpbx.py
 	#xcodebuild -target install
 	#xcodebuild -target blender

Because the python module is developed earlier than cmake,
it will complain `warning: unknown PBX type: PBXBuildStyle`,
that's OK to continue without problem.

The target 'install' is to install 'self-carried' lib of Blender,
which can not be omitted.

#####note: Adding option(s) can be like this:

	#cmake ../blender -GXcode -DWITH_CYCLES_DEBUG=ON
	
#####note: macOS Sierra has removed QuickTime Framework, which means all `#import <QTKit/QTKit.h>` will hit compilation error. The workaround is to add -DWITH_CODEC_QUICKTIME=No option to cmake:

	   #cmake ../blender -GXcode -DWITH_CYCLES_DEBUG=ON -DWITH_CODEC_QUICKTIME=No
	   
