layout: post
title: "Pass by Value/Reference of Struct/Class in Swift"
date: 2014-06-16 08:19:18 +0800
comments: true
Category: Ios
Tags: Swift, Pass By Value, Pass By Reference
Has_Math: True


By Swift Document, the assignment of `Class` is pass by reference, and the `Struct` is pass by value.

Here we make a simple experiment.
<!--More-->
So, if we __assign__ a struct var, all the __corresponding fields__ in the original struct are __copied__.

What if it contains a class? Here is the experiment:

	:::Objective-C

	// Playground - noun: a place where people can play

	import UIKit

	struct Resolution {
	    var width = 0
	    var heigth = 0
	}

	class VideoMode {
	    var resolution = Resolution()
	    var interlaced = false
	    var frameRate = 0.0
	    var name: String?
	}

	struct Display {
	    var videoMode: VideoMode // struct contains a class
	    var brand: String
	    var responseTime: Double
	}


	var myDisplay = Display(videoMode: VideoMode(), brand: "Asus", responseTime: 2.0)

	// struct assignment, each field is copied
	var yourDisplay = myDisplay

	// experiment about the VideoMode class in the Display struct
	yourDisplay.videoMode.interlaced = true

	myDisplay.videoMode.interlaced
	// true



Here is the simple diagram :

![Swift class in struct](http://coding-addict.com/pictures/blogger/swift class in struct.png)

So the structure assignment is simply __copying__, even for the class reference.
#
