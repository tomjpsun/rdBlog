layout: post
title: "Embed the UIWebView in UIScrollView using AutoLayout"
date: 2015-07-31 16:51:16 +0800
comments: true
Category: Ios
Tags:Uiscrollview, Autolayout, Uiwebview
Has_Math: True


有時候會遇到這樣的需求：將 UIWebView 嵌入 UIScrollView.

`這裏的重點在於` [Apple UIWebView Document 的特別注意事項:](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIWebView_Class/)

	Important

	You should not embed UIWebView or UITableView objects
	in UIScrollView objects.
	If you do so, unexpected behavior can result because
	touch events for the two objects can be mixed up
	and wrongly handled.

就是*手勢滑動會有問題*. 所以權宜之計就是把 UIWebView scroll 屬性給 disable.

<!--More-->

`另外一個重點`，就是 Layout constraint 的依賴關係. 因為 UIScrollView contentSize 會依賴於 subviews, 比較清楚的做法就是弄一個 container view 當作 UIScrollView 唯一的 subview, 其他原本的 sub views 都放在 container view 裡面, 所以 UIScrollView contentSize 就只依賴這個 container view 的大小. 於是, container view 內嵌入 UIWebView, 這裏又遇到一個問題, container view 的寬, 要與 iOS 裝置等寬, 長度要包覆 UIWebView 的長度, 也就是要看 UIWebView 載入資料後才決定, 如果我們用 AutoLayout 解決這個問題, 有兩個部分需要比較特別的方式：

* 寬度 layout 要「跳級」拿 UIController 的 UIView 來參考, 這個 UIView 就是 UIScrollView 的 parent view. 也就是在 viewDidLoad 裡面加上 constraint:

		:::objective-c

	    NSLayoutConstraint *leftEdgeAlign =
	    [NSLayoutConstraint constraintWithItem:self.contentView
	     attribute:NSLayoutAttributeLeading relatedBy:0 toItem:self.view
	     attribute:NSLayoutAttributeLeft multiplier:1.0 constant:0];

	    [self.view addConstraint:leftEdgeAlign];

	    NSLayoutConstraint *rightEdgeAlign =
	    [NSLayoutConstraint constraintWithItem:self.contentView
	    attribute:NSLayoutAttributeTrailing relatedBy:0 toItem:self.view
	    attribute:NSLayoutAttributeRight multiplier:1.0 constant:0];

	    [self.view addConstraint:rightEdgeAlign];



* 長度 layout 要在 UIWebView delegate (一般是給 UIViewController) 裡面的 webViewDidFinishLoad:(UIWebView *)webView 才決定, 這時有個__長度不準確__的問題,
在 [StackOverflow](http://stackoverflow.com/questions/19507639/uiwebview-doesnt-load-completely-height-varies) 有討論到, 大意是 [webView sizeToFit] 得到的
size 是不正確的, 原因仍不明. 但是,這樣會造成 container view 會 clip 掉 web view 後面的內容, work around 做法, 用 javascript 可以得到正確的 size:


		:::objective-c
		@interface WebDetailViewController() @property (weak, nonatomic) IBOutlet NSLayoutConstraint *webViewConstraintHeight;
		@end
		(void)webViewDidFinishLoad:(UIWebView *)webView {
			CGFloat height = [[webView stringByEvaluatingJavaScriptFromString:@"document.height"] floatValue];
			CGFloat width = [[webView stringByEvaluatingJavaScriptFromString:@"document.width"] floatValue];
			CGRect frame = webView.frame;
			frame.size.height = height;
			frame.size.width = width;
			webView.frame = frame;
			self.webViewConstraintHeight.constant = self.webView.frame.size.height;
			 webView.hidden = false;
		}


其中, webViewConstraintHeight 可以用 IB 拉出來：

<img src="http://coding-addict.com/pictures/rd/layout constraint drag connect.png" alt="Drawing" style="width: 2400px;"/>

網路上這裏整理的很好：
[羅國輝 的 AutoLayout 系列](http://grayluo.github.io/WeiFocusIo/category.html#autolayout-ref)

另外 Apple 官方的文件, 解釋得不如上面清楚：
[Tech Note TN2154](https://developer.apple.com/library/ios/technotes/tn2154/_index.html)


