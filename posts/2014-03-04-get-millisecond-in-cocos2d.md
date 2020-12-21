layout: post
title: "get millisecond in Cocos2d"
date: 2014-03-04 18:16:50 +0800
comments: true
Category: Cocos2D
Tags:Timer, Millisecond
Has_Math: True

People use timer for many things in Game. How do we get the special need for milli-second precision ? In a Cocos2d game, there are 2 ways suggested by people in the [StackOverflow](http://stackoverflow.com/questions/358207/iphone-how-to-get-current-milliseconds):
<!-- more -->
The first is

	:::c
	double CurrentTime = CACurrentMediaTime();

Since `NSDate draws from the networked synch-clock` and it sometimes may 'lag'.

The second is to overwrite the method of CCNode:

	:::c
	- (void)update:(CCTime)delta {
		totalTime += delta;
	}

The above method is by default called by the scheduler, or you can schedule yourself, see [schedule your own selector](http://www.koboldtouch.com/display/IDCAR/Scheduling+Updates+and+Selectors).


#
