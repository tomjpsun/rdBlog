layout: post
title: "CoreAudio Header Files"
date: 2013-11-02 06:56
comments: true
Category: Ios
Tags: Core Audio
Has_Math: True

If you get "cannot find CADebugPrintf.h" message on compiling some demo projects from Apple related to audio, this short note may help.


<!-- more -->

* Download and open the demo project "Core Audio Utility Classes"

* Copy the directory "CoreAudio" to any path you like. I use "/Developer/Extras", which has been dropped since Xcode 4, where I can still use without problem.

* Add it to the "Header Search Paths" in your project, e.g.: In my project configuration,  I use "/Developer/Extras/CoreAudio/PublicUtility"

* Done!

