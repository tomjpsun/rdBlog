layout: post
title: "convert iPhone storyboard to iPad"
date: 2014-01-18 12:16:00 +0800
comments: true
Category: Ios
Tags: Storyboard
Has_Math: True

This post is an abstract of the [answers on the StackOverflow](http://stackoverflow.com/questions/8465769/converting-storyboard-from-iphone-to-ipad).

1.Duplicate your iPhone-Storyboard and rename it MainStoryboard_iPad.storyboard

2.right click on the storyboard -> “open as” -> “Source Code”.

3.Search for targetRuntime=`"iOS.CocoaTouch"`and change it to `targetRuntime="iOS.CocoaTouch.iPad"`

5.Search for `<simulatedScreenMetrics key="destination" type="retina4"/>` and change it to to `<simulatedScreenMetrics key="destination"/>`

