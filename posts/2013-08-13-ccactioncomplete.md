title: "CCActionComplete"
date: 2013-08-13 16:50
Category: Cocos2D
Has_Math: True


使用 CCAction 時，若希望在 action 完成後，才執行某些動作？

因為 CCAction 是非同步執行，也沒有直接的方法設定 completion handler。 可以借用 CCSequence 依序執行的特性，放置一個 call back function (CCCallbackFunc) 來完成這樣的事：
<!-- more -->

	:::Objective-C

	CCSequence *seq = [CCSequence actions:

	[CCMoveTo actionWithDuration:3.0
              position:ccp(100, 100)],

	[CCCallFunc actionWithTarget:self
              selector:@selector(moveComplete)], nil];

	[sprite runAction:seq];



安排的 completion handler 就可以這樣寫了：

	- (void)moveComplete{ … }

[參考出處](http://www.cocos2d-iphone.org/forum/topic/20550)
