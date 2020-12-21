layout: post
title: "Python Class Info"
date: 2015-08-28 16:39:15 +0800
comments: true
Category: Python
Tags: Classinfo
Has_Math: True

I am a python newbie, to help me inspect the type of some vars, this is very handy:

	:::python
	iVar = ...(result from API)
	print(iVar.__class__.__name__)

Today I solve a problem with the help of an `online regex` site:

[https://regex101.com/#python](https://regex101.com/#python)

Which has good explanation and let me understand what's wrong with my regex.

To inspect the result of regex, we can also `print` it !!

	:::python
	#import re
	...
	match = re.search(pattern, str)
	print (match)

#

