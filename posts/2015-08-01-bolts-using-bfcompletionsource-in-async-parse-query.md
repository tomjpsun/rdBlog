layout: post
title: "Bolts: Using BFCompletionSource In Async. Parse Query"
date: 2015-08-01 16:44:13 +0800
comments: true
Category: Ios
Tags:Bolts, Bftask
Has_Math: True

Bolts Framework 是一個 Promise Pattern 的實作, 我在使用上遇到一個撞牆的問題, 特別紀念一下.

<!--More-->

因為這個 Framework 寫得很好, 使用問題不多 所以網路上再怎麼找, 都是指到 Bolts 的使用說明, 我的需求是要寫一個 async. query, 所以參考到的, [都是這個 Bolts 的範例](https://github.com/BoltsFramework/Bolts-iOS/blob/master/Readme.md#creating-async-methods).

	:::Objective-C
	 (BFTask *) fetchAsync:(PFObject *)object {
	 BFTaskCompletionSource *task = [BFTaskCompletionSource taskCompletionSource];
	 [object fetchInBackgroundWithBlock:^(PFObject *object, NSError *error) {
	   if (!error) {
	     [task setResult:object];
	   } else {
	     [task setError:error];
	   }
	 }];
	 return task.task;


照抄到我的部分, 變成：

	:::Objective-C
	- (BFTask *)queryStation
	{
	    BFTaskCompletionSource *source =
	    [BFTaskCompletionSource taskCompletionSource];
	    PFQuery *query = [PFQuery queryWithClassName:@"Station"];
	    [query whereKey: @"name" containsString: @"xx電台"];
	    query.cachePolicy = kPFCachePolicyCacheThenNetwork;

	    [query findObjectsInBackgroundWithBlock:^(NSArray *objects, NSError *error) {
	        if (!error) {
	            [source setResult: self.station];

	        } else {
	            [source setError: error];
	        }

	    }];
	    return source.task;
	}



但是會一直引發 Exception:

	:::Objective-C

	- (void)setResult:(id)result {
	    if (![self trySetResult:result]) {
	        [NSException raise:NSInternalInconsistencyException
	                    format:@"Cannot set the result on a completed task."];
	    }
	}


撞牆的原因就是 `kPFCachePolicyCacheThenNetwork` 這個 flag 導致執行這部分兩次, 因為第一次已經設定 BFTask result, 所以第二次就會 raise exception, 將 flag 改為 `kPFCachePolicyNetworkThenCache` 可以使 query 先行找網路再找 cache, 也就是僅執行這裏一次, 就不會 raise exception 了.#


