layout: post
title: "Let Apple TestFlight Sent Invitation"
date: 2015-02-04 06:09:21 +0800
comments: true
Category: Ios
Tags:Testflight
Has_Math: True

Apple 併購 TestFlight 之後，漸漸整合到它的 iTunes Connect，有些改變是需要注意的：

* 安裝在手機上的 TestFlight App 改成由 Apple 提供，Icon 從原來的六片變三片，長這樣：

<img src="https://devimages.apple.com.edgekey.net/assets/elements/icons/96x96/testflight_2x.png" alt="Drawing" style="width: 80px;"/>

* 加入測試者，透過 `iTunes Connect | 使用者和角色` 來管理

* 對於已經加入的測試人員，如果需要重新發 email 邀請，務必記得到 `我的 App | 預先發行 | 內部版本 | 右側 TestFlight 測試版測試` 重新 toggle switch 一次：

<img src="http://coding-addict.com/pictures/blogger/apple testflight trigger email sent.png" alt="Drawing" style="width: 800px;"/>

#
