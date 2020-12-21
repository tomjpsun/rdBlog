layout: post
title: "Application-identifier entitlement does not match"
date: 2016-05-09 10:35:14 +0800
comments: true
Category: Ios
Tags: Debug
Has_Math: True

最近遇到一個 iOS build problem:

	This application's application-identifier entitlement
	does not match that of the installed application.

<img src="http://coding-addict.com/pictures/rd/ios_build_problem.png" alt="Drawing" style="width: 400px;"/>

解決方法是開啟 Xcode Device Window(在 Window 選單裡面)

左側選擇我們的 iPhone，右側 刪除 app 後，

重新 build & download 即可！

<img src="http://coding-addict.com/pictures/rd/ios_build_problem_Xcode device window.png" alt="Drawing" style="width: 800px;"/>


#
