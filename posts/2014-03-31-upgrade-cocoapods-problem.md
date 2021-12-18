title: "upgrade Cocoapods problem"
date: 2014-03-31 11:49:35 +0800
comments: true
Category: Ios
Tags: Cocoapods
Has_Math: True

After upgrading CocoaPods 0.29 -> 0.30, I encounter a strange error which has no furthur solution hint from Googling.
<!-- more -->
It is a ruby error complaining for `no definition for default enforce_available_locales value`...

After several hours of trying, this kind of error may be caused by the rails.

	sudo gem install rails

So, after upgrading rails 4.0.1 to 4.0.4. It DID solved the error, even though I don't know why.

#
