layout: post
title: "The API differences of NSRegularExpression"
date: 2014-01-23 07:06:22 +0800
comments: true
Category: Ios
Tags: Nsregularexpression
Has_Math: True

##Background
We have a text file like this:

	checker1.png
	checker2.png
	checker3.png
	checker4.png
	checker5.png
And we want to enumerate each name by NSRegularExpression, code snippet like this:
<!-- more -->

	:::Objective-C

    NSString *strFileContent = [NSString stringWithContentsOfFile:resourcePath
                                                         encoding:NSUTF8StringEncoding error:&error];

    NSRegularExpression *regex = [NSRegularExpression regularExpressionWithPattern:@"^.*$" options:NSRegularExpressionAnchorsMatchLines error:&error];

    NSMutableArray *recorder = [NSMutableArray array];

    [regex enumerateMatchesInString:strFileContent
                            options:NSMatchingReportProgress
                              range:NSMakeRange(0, [strFileContent length])
                         usingBlock:^(NSTextCheckingResult *result, NSMatchingFlags flags, BOOL *stop) {

        NSString *matchedStr = [strFileContent substringWithRange: result.range];

        // exclusive empty strings
        if (![matchedStr isEqualToString:@""]) {
            [recorder addObject: matchedStr];
        }
    }];
    return [recorder copy];



Since we are using `NSMatchingReportProgress` options in the `[regex enumerateMatchesInString:...]`,
we can get the result range enumeration like this:

	(0,12)
	(0,0)
	(13,12)
	(0,0)
	(26,12)
	(0,0)
	(39,12)
	(0,0)
	(52,12)

Why there are `(0,0)s`? Well, I don't know, but we have to work-around the output --- to exclude
empty strings before return like the previous code snippest.

##Improvement

In Apple Document, I found another `[NSRegularExpression matchesInString:options:range:]`. The
Document says

		This is a convenience method that calls enumerateMatchesInString:options:range:usingBlock:...

OK, refactor the previous code snippest like this:

	:::Objective-C

    NSString *strFileContent = [NSString stringWithContentsOfFile:resourcePath
                                                         encoding:NSUTF8StringEncoding error:&error];

    NSRegularExpression *regex = [NSRegularExpression regularExpressionWithPattern:@"^.*$" options:NSRegularExpressionAnchorsMatchLines error:&error];

    NSArray *matchedesult = [regex matchesInString:strFileContent options:NSMatchingReportProgress range: NSMakeRange(0, [strFileContent length])];

    NSMutableArray *fileNames = [NSMutableArray array];

    for (NSTextCheckingResult *aResult in matchedesult) {

        [fileNames addObject: [strFileContent substringWithRange: aResult.range]];
    };


This time we get an array of NSTextCheckingResult, extract each range we get:

	(0,12)
	(13,12)
	(26,12)
	(39,12)
	(52,12)

This is just we need, no work-around needed.
#
