layout: post
title: "self made Dash document with doxygen"
date: 2014-02-14 13:54:38 +0800
comments: true
Category: Misc
Tags: Dash, Cocos2D-Iphone
Has_Math: True

The [Dash](http://kapeli.com/dash) document is a handy tool for API reference. There are lots of ready-to-download documents. I use it to  learn the cocos2d-iPhone APIs.

But I have encounter a situation, which, after fetching the most updated source, I want to build the API references from them, and I want to use Dash to browse them. The following are the steps to accomplish such needs:
<!-- more -->
###Prepare Environment###
* Download, build, and install Doxygen [here](http://www.stack.nl/~dimitri/doxygen/download.html)
* Download Graphiz executable package [here](http://www.graphviz.org/Download..php)

Build Doxygen from source will install command line in /usr/local/bin/doxygen.

Installing Graphiz package will also install command line tools and the GUI App at the same time.

###Make Doc and Browse###
In `cocos2d-iPhone` directory, change doxygen.config settings:

	GENERATE_DOCSET   = YES
	DISABLE_INDEX     = YES
	SEARCHENGINE      = NO
	GENERATE_TREEVIEW = NO

The upper settings will generate a clean, easy to read Doc. Suggest you modify the default one.
e.g.:The default settings `GENERATE_TREEVIEW = ALL` will make a redundent side view since Dash already have one.

* In `cocos2d-iPhone` directory, run `doxygen doxygen.config`
* Go to `html/` subdirectory, then run `make`
* `tar --exclude='.DS_Store' -cvzf cocos2d-iPhone.tgz com.sapusmedia.Cocos2d.docset`
* With GUI file manager, open the `com.sapusmedia.Cocos2d.docset` with the Dash App, it will be imported at the same time.

The above commands is referenced from the Dash [help](http://kapeli.com/docsets).

#
