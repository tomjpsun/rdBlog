layout: post
title: "Development with UIKit Dynamics"
date: 2014-04-04 08:29:19 +0800
comments: true
Category: Ios
Tags: Uikit
Has_Math: True



##一些 references:##

- UIKit Dynamics 是 iOS7 推出的 framework，目的是模擬一些 UI 元件的物理效果。一個很不錯的例子，就是[這裡的 sidebar menu](http://www.teehanlax.com/blog/introduction-to-uikit-dynamics/)。

- Xcode 文件的範例 DynamicsCatalog 提供了很好的學習範例，把 UIKit Dynamics 的 Classes 做了很入門的介紹。
<!-- more -->
- WWDC 2013 有兩個 track 也是很好的起始：

		206 - Getting Started with UIKit Dynamics
		221 - Advanced Techniques with UIKit Dynamics

##突發奇想##

既然 sidebar menu 都可以用 UIKit Dynamics 來模擬，那麼我想要弄一個 2 thumbs range slider 應該也不是問題吧？

Source Code is on the GitHub:

[https://github.com/tomjpsun/TestUIDynamics](https://github.com/tomjpsun/TestUIDynamics)

##遇到的麻煩##

<iframe width="420" height="315" src="http://www.youtube.com/embed/dkVDl514y1I" frameborder="0" allowfullscreen></iframe>

如同影片中的情形，UIAttachmentBehavior 必須要搭配 damping，否則該 item 會「黏」在牆壁上。 但是 damping 無論設定任何值都會使得 attachment 看起來像是彈簧，而不是剛性的附著在 gesture touch point 上面。簡言之，UIKit Dynamics 對於剛性的 attachment 支援不佳。當然，軟體無所不能，經過複雜的 tweak 還是會做得出來，但是已經失去了採用這個 framework 方便完成任務的本意，就停留在這兒了！

##額外的試驗##
DynamicsCatalog 的 sample code，把 Collision + Gravity + Spring 範例改一下 offset:

	:::Objective-C
	//UIOffset attachmentPoint = UIOffsetMake(-25.0, -25.0);
	UIOffset attachmentPoint = UIOffsetMake(0.0, 0.0);
	// By default, an attachment behavior uses the center of a view. By using a
	// small offset, we get a more interesting effect which will cause the view
	// to have rotation movement when dragging the attachment.
	UIAttachmentBehavior *attachmentBehavior = [[UIAttachmentBehavior alloc] initWithItem:self.square1 offsetFromCenter:attachmentPoint attachedToAnchor:squareCenterPoint];
	...
	//self.square1AttachmentView.center = CGPointMake(25.0, 25.0);
	self.square1AttachmentView.center = CGPointMake(CGRectGetMidX(self.square1.bounds), CGRectGetMidY(self.square1.bounds));
	...
	//[(APLDecorationView*)self.view trackAndDrawAttachmentFromView:self.attachmentView toView:self.square1 withAttachmentOffset:CGPointMake(-25.0, -25.0)];
	[(APLDecorationView*)self.view trackAndDrawAttachmentFromView:self.attachmentView toView:self.square1 withAttachmentOffset:CGPointMake(.0, .0)];


這樣改，把 attach 點放在物件中心，移動時比較不會亂轉。
然後隨便碰牆壁幾下就可以看到「黏」在牆壁上的現象。

#
