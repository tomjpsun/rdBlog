layout: post
title: "追半天的打字造成的 bug"
date: 2014-03-10 16:16:00 +0800
comments: true
Category: Ios
Tags: Debug
Has_Math: True

今天花了半天，追出一個 type bug!
<!-- more -->
* * *

HotViewController 使用 KVO 追蹤 Model 物件的播放狀態：

In HotViewController.m:

	:::Objective-C
	- (void)viewDidLoad
	{
		...
		[[RODModel shareInstance] addObserver:self
		forKeyPath:@"playingStatus"
		options:NSKeyValueObservingOptionNew
		context:PlayingStatusIsUpdatedNotification];
		...
	}


	:::Objective-C
	- (void)dealloc
	{
    	[[RODModel shareInstance] removeObserver:self
    	forKeyPath:@"playingStats"];
	}



結果發現 dealloc: 時候會 crash !

原因是 playing`Stats` 少打一個字母: playing`Status` ，很無言！

心得：以後還是乖乖地將它定義常數，然後借用 Xcode Autocomplete 來完成，就可以避免這類的錯誤！
