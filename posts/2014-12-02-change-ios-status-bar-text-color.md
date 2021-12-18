layout: post
title: "Change iOS status bar text color"
date: 2014-12-02 15:48:25 +0800
comments: true
catetory: iOS
Tags: Statusbar
Has_Math: True

如果要對 UINavigationBar 給特製底圖時，會遇到 status bar text 暗色的問題，就像這樣：

<img src="http://coding-addict.com/pictures/blogger/dim status bar text.png" alt="Drawing" style="width: 400px;"/>

<!--More-->

兩個步驟可以使 status bar text 變成白色：

1. info.plist 加上 key:`UIViewControllerBasedStatusBarAppearance` value:`NO`
2. `[AppDelegate didFinishLaunchingWithOptions:]` 加上設定：

{% codeblock  lang: objc %}

		[application setStatusBarHidden:NO];
		[application setStatusBarStyle:UIStatusBarStyleLightContent];

{% endcodeblock %}

這時候應該可以變成這樣：

<img src="http://coding-addict.com/pictures/blogger/light status bar text.png" alt="Drawing" style="width: 400px;"/>

#

