layout: post
title: "Xcode vector assect tips"
date: 2014-12-02 18:17:14 +0800
comments: true
Category: Ios
Tags:Xcode
Has_Math: True

Xcode 6 已經可以輸入 vector image 了，只要在 project navgator 裡面打開 Images.xcassets，然後直接把 pdf 拉進來，就如下圖所示：

<!--More-->

<img src="http://coding-addict.com/pictures/blogger/vector asset still not ready.png" alt="Drawing" style="width: 600px;"/>

這時候，還要多做一動：就是把 image 拉到框框裡，像這樣才算完工了：

<img src="http://coding-addict.com/pictures/blogger/vector asset ready.png" alt="Drawing" style="width: 600px;"/>

不然 App 會找不到圖喔！

在程式碼中，載入這些圖檔的方式，跟以前相同：
{% codeblock lang:objc %}

		UIImage* bg_nav = [UIImage imageNamed:@"navigator_background"];

{% endcodeblock %}
這樣就 OK 了！
