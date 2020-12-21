layout: post
title: "iOS 系統取用限制 密碼忘記"
date: 2015-09-01 14:43:19 +0800
comments: true
Category: Ios
Tags: Password
Has_Math: True

如果有人向筆者一樣，記性很不好，忘記了 iOS 系統設定裡面 的 取用限制 密碼，那麼這篇可以當作參考。

<!--More-->

<img src="http://coding-addict.com/pictures/rd/ios-restriction-on.png" alt="Drawing" style="width: 200px;"/>

如果有設定密碼，就會跳出詢問，輸入錯誤密碼會得到這樣的結果：

<img src="http://coding-addict.com/pictures/rd/error-password.png" alt="Drawing" style="width: 200px;"/>

使用 iTunes 回復備份，也不能解決這個問題。


首先下載這個工具:

[iPhone Backup Extractor](http://www.iphonebackupextractor.com/)

打開後

然後 Extract `/Home Domain/Library/Preferences/com.apple.restrictionspassword.plist`

<img src="http://coding-addict.com/pictures/rd/iPhone Backup Extractor.png" alt="Drawing" style="width: 800px;"/>

使用文字編輯器 編輯那個 plist:

	:::html
	<?xml version="1.0" encoding="UTF-8"?>
	<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
	<plist version="1.0">
	<dict>
		<key>RestrictionsPasswordKey</key>
		<data>
		WHuDS6oCUmmWJ0Z9Ht80ODgQGPE=
		</data>
		<key>RestrictionsPasswordSalt</key>
		<data>
		9/9SHA==
		</data>
	</dict>
	</plist>

將 兩個 `data`  分別貼入
[這個比對的網頁](http://ios7hash.derson.us/)

以上面的資料為例子，就這樣填寫：

<img src="http://coding-addict.com/pictures/rd/ready to search password.png" alt="Drawing" style="width: 600px;"/>

按下 `search for code` 請他從 0000 開始暴力破解到 9999，即可算出忘記的密碼。

註：依照網頁的說明，目前支援 iOS 7,8 #
