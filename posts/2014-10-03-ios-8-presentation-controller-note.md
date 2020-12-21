layout: post
title: "iOS 8 Presentation Controller Note"
date: 2014-10-03 16:44:37 +0800
comments: true
Category: Ios
Tags: Presenting View Controller
Has_Math: True

iOS 8 新推出的 Presentation Controller. 將 View Controller 過場(transition) 分工合作，將過場的動作，交給 Presentation Controller 來打理，原先的 View Controller 稱為 __presenting__ View Controller, 過場後出現的 View Controller 稱為 __presented__ View Controller.

<!--More-->

官方資料在 WWDC 2014 Session 228 "A Look Inside Presentation Controllers"

官方範例在[這裡](https://developer.apple.com/library/ios/samplecode/LookInside/Introduction/Intro.html#//apple_ref/doc/uid/TP40014643-Intro-DontLinkElementID_2)

另外找到還不錯的[範例](http://dativestudios.com/blog/2014/06/29/presentation-controllers/), 雖然是用 swift 寫的，但是非常簡單扼要，值得一看。

![](/images/ModalViewTransitioning.png)


內心 OS: Apple 啊卡好，一整天就搞這個了 ~
