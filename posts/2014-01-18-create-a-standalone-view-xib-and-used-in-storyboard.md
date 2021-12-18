layout: post
title: "create a standalone view .xib and used in storyboard"
date: 2014-01-18 12:29:34 +0800
comments: true
Category: Ios
Tags: Storyboard, Ui, Xib
Has_Math: True

Xcode IB (Interface Builder) 把許多 UI 以視覺化的方式讓使用者設定。這省去的許多程式碼的撰寫。在 iOS 5 之後，View Controllers 的界面佈局（即多個 .xib files) 都以一個新的 storyboard file 來整合。但是有些時候，我們還是想要單獨的針對一個 UIView 來佈置 UI。當然 Xcode 5 本來就是允許 storyboard 與 .xib 並存的。以下整理的資料來源出自 StackOverflow:

<!-- more -->

[創建 standalone xib](http://stackoverflow.com/questions/14139564/create-a-standalone-view-in-storyboard-to-use-programatically)

[調用 創建好的 nib 文件](http://stackoverflow.com/questions/863321/iphone-how-to-load-a-view-using-a-nib-file-created-with-interface-builder?rq=1)

![new view for xib](http://coding-addict.com/pictures/blogger/new view for xib.png)
創建 xib 步驟：

    1.Xcode 選擇 File | New | File
    2.選擇 User Interface | View
    3.選擇 Next | Next | 爲 View 取新名稱
    4.新的 .xib 就會產生，可以編輯了。

如果需要自定義 view 的 size，可以在 Attribute Inspector | Simulated Metrics | Size 選擇 "Freeform"：

![free form view](http://coding-addict.com/pictures/blogger/free form view.png)

編好的 .xib 如下圖，可以看到與 storyboard 並存於同一個 project：
![xib and storyboard co-exist](http://coding-addict.com/pictures/blogger/xib and storyboard co-exist.png)

載入 xib，範例：

	:::Objective-C

	EpisodeAbstractView *descView =
	[[[NSBundle mainBundle] loadNibNamed:@"EpisodeAbstractView"
	owner:self options:nil] objectAtIndex:0];


此處 "EpisodeAbstractView" 就是 .nib 檔案名稱。 `loadNibNamed:` 會得到一個由 .nib 所有 Views 組成的 Array，這裡我們只 create 一個，因此取出唯一的 View 即是。

For a complete example, please refer to [CustomXib project on GitHub](https://github.com/tomjpsun/CustomXib)
#


