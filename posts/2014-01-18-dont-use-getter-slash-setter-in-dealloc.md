layout: post
title: "DONT use getter/setter in dealloc"
date: 2014-01-18 17:56:37 +0800
comments: true
Category: Objc
Tags: Getter/Setter
Has_Math: True

	:::Objective-C

	-(void) dealloc
	{
	  [name release]; // instead of [[self name] release]   ...
	  [super dealloc];
	}


The reason to avoid accessors in dealloc is that - if there are observers or an override in a subclass that triggers behavior, it'll be triggered from dealloc which is pretty much never what you want (because the state of the object will be inconsistent).
#
