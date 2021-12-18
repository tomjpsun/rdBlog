layout: post
title: "Dirty Interface Builder Handler"
date: 2014-12-02 17:59:23 +0800
comments: true
Category: Ios
Tags: Interface Builder
Has_Math: True


今天解決一個問題：使用 IB 建立 UI 物件後，遇到了 Crash，抱怨說 `unrecognized selector sent to instance xxxxxx`，如下圖：
<!--More-->

![](/images/non-existed object cause crash.png)

這時記得趕快檢查，是否曾經在 IB 裡面拉過線，然後對應物件已經不存在的情形，如下圖示：

![](/images/dirty handler connection in IB.png)

這裏發現 `Value Change` 是以前拉的連線，後來經過程式碼的變動，該對應物件已經不會產生，所以執行時就直接 crash，只要 unlink 這個地方就解決了。

#
