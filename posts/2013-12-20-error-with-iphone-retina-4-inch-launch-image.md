layout: post
title: "Error with iPhone retina 4-inch Launch image"
date: 2013-12-20 17:50:57 +0800
comments: true
Category: Ios
Tags: Xcode
Has_Math: True

While Archiving in Xcode5, I see the error messages:

	While reading /Users/admin/Desktop/Projets tests/myProject/Resources/build/Default-568h@2x.png pngcrush caught libpng error:   Not a PNG file
	Could not find file: /Users/admin/Library/Developer/Xcode/DerivedData/myProject-fjnhmdxkawhvkgecsmmrbgcwqaxk/Build/Products/Debug-iphoneos/myProject.app/Default-568h@2x.png
	Command /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/usr/bin/copypng emitted errors but did not return a nonzero exit code to indicate failure

<!-- more -->

##Launch Image Incorrect ?? How to solve ??

###Delete old "Launch Images":

Xcode5 > Targets > General > Launch Images > small arrow circle > keyboard 'delete'

![result](http://coding-addict.com/pictures/blogger/Xcode5 Launch Images.png)

###Add new one:

Launch Images now has a menu list, select *"Don't use asset catalogs"*, then menu changed to *"use Asset Catalog"*, **select again**, the migrate dialog will appear, select *"migrate"*.

![result](http://coding-addict.com/pictures/blogger/Xcode5 migrate.png)

###Drag your Launch Images:

now select the small circle, drag & drop your launch images.

After the workaround, **Xcode5 > Product > Archive** should be OK!
