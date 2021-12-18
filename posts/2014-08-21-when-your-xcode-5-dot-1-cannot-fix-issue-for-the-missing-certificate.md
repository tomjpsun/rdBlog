layout: post
title: "When your Xcode 5.1 cannot fix issue for the missing certificate"
date: 2014-08-21 17:50:58 +0800
comments: true
Category: Ios
Tags: Certificate
Has_Math: True

下載 App 到手機，有時候會遇到一些黃三角警告，Xcode 5.1 都會自動 fix ，但是當這種的情形，Xcode 5.1 一直無法 Fix issue。

![No matching provisioning profile](http://coding-addict.com/pictures/blogger/no matching provisioning profiles found.png)

<!--More-->

解決方法，就是到我們的 iOS developer site，__Revoke Certificate__，與砍掉__相關的__ Provision Profile，（對！相關的都要砍乾淨）。然後回 Xcode 重新 Fix issue，理論上，就會重新 request 一個 certificate，然後自動產生所需的 Provision Profile。


如果 Xcode 抱怨 "can not find certificate"，或許要登入 iOS developer site 確認我們的 certificate 在不在，確認方式如下：

* 進入 Keychain App，看看所需要的 certificate 應該要在，否則就要重新申請一份。

![iPhone Developer Certificate](http://coding-addict.com/pictures/rd/iPhone Developer Certificate.png)
在 keychain access 找到相關的 certificate（檢查到期日相同）


![http://coding-addict.com/pictures/rd/apple developer site-provisioning profiles.png]("http://coding-addict.com/pictures/rd/apple developer site-provisioning profiles.png")
在 iOS developer site 準備 editing 相關的 provision profile

* Certificate 確認後，就到 iOS developer site 確認是否有 Provisioning Profile 包含這個 certificate?（檢查 App ID 是否包含我們的 App)


<img src="http://coding-addict.com/pictures/rd/edit iOS provisioning profile.png" alt="Drawing" style="width: 500px;"/>

* 好了後按 `Generate` 然後下載匯入 Xcode，應該可以解決問題。

p.s. 再不行就放大絕：進 KeyChain App 把 Certificate 砍掉，然後進 Xcode Fix issue 讓他全部自動重新產生。
