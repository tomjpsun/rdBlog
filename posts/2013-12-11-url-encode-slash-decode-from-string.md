title: "URL encode/decode from string"
date: 2013-12-11 08:59:06 +0800
comments: true
Category: Ios
Tags: Url Encode
Has_Math: True

看到這一篇整理 [如何轉換 URL Encode/Decode](http://cybersam.com/ios-dev/proper-url-percent-encoding-in-ios) 的文章，覺得很實用，趁此記錄下來。

<!-- more -->

這篇的引文是這樣：使用 ```stringByAddingPercentEscapesUsingEncoding:NSUTF8StringEncoding``` 可以 encode URL，例如：


	Original text: One Broadway, Cambridge, MA
	Encoded text: One%20Broadway,%20Cambridge,%20MA


文中的重點是這句：```stringByAddingPercentEscapesUsingEncoding``` doesn’t encode reserved characters like ampersand (&) and slash (/).

以下這個例子就是無法轉 & 與 / 的情形：

	Original text: Bed Bath & Beyond – URL=http://www.bedbathandbeyond.com/
	Encoded text: Bed%20Bath%20&%20Beyond%20-%20URL=http://www.bedbathandbeyond.com/

所以要用```CFURLCreateStringByAddingPercentEscapes```來做，實際的例子像這樣：

	:::Objective-C

	// Encode a string to embed in an URL.
	NSString* encodeToPercentEscapeString(NSString *string) {
	return (NSString *)CFURLCreateStringByAddingPercentEscapes(NULL,
                                                               (CFStringRef) string,
                                                               NULL,
                                                               (CFStringRef) @"!*'();:@&=+$,/?%#[]",
                                                               kCFStringEncodingUTF8);
                                                           }



轉換結果：

	Original text: Bed Bath & Beyond – URL=http://www.bedbathandbeyond.com/
	Encoded text: Bed%20Bath%20%26%20Beyond%20-%20URL%3Dhttp%3A%2F%2Fwww.bedbathandbeyond.com%2F

大部份的時機，只有 HTTP 參數才會碰到這些轉換，但是從這個例子還是可以觀察到 & 與 / 有被轉換到。

註：[Apple 的相關文件](https://developer.apple.com/library/ios/documentation/cocoa/Conceptual/URLLoadingSystem/WorkingwithURLEncoding/WorkingwithURLEncoding.html#//apple_ref/doc/uid/10000165i-CH12-SW1)
