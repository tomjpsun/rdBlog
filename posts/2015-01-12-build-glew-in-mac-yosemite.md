layout: post
title: "Build GLEW in Mac Yosemite"
date: 2015-01-12 09:39:29 +0800
comments: true
Category: Opengl
Tags:Build,Glew
Has_Math: True

[GLEW](http://glew.sourceforge.net/) 是 build OpenGL 時，常常搭配 glut, glfw 的輔助 library，但是筆者從 他的 GitHub 下載時會少了 glew.c 檔案，所以改用他的 .zip source 來 build.

本篇撰寫時，最新版本為 glew-1.11
<!--More-->
準備動作，需要有 Xcode(筆者環境是 Xcode 6.1)

	sudo xcode-select -s /Applications/Xcode.app/Contents/Developer/

觀察 glew 目錄的 Makefile，發現

	GLEW_PREFIX ?= /usr
	GLEW_DEST ?= /usr
	BINDIR    ?= $(GLEW_DEST)/bin
	LIBDIR    ?= $(GLEW_DEST)/lib
	...

就是預設安裝在 /usr/lib 下的意思，這樣會使用到 sudo，這裏不想使用預設 path，所以 build command 為：

	make GLEW_DEST=/usr/local install

這樣會安裝在 /usr/local/lib 下，結果為：

	Mac-mini:glew-1.11.0 tom$ ls /usr/local/lib/libGLEW.*
	/usr/local/lib/libGLEW.1.11.0.dylib	/usr/local/lib/libGLEW.a
	/usr/local/lib/libGLEW.1.11.dylib	/usr/local/lib/libGLEW.dylib

以上。
