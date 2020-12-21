layout: post
title: "Parse cache policy"
date: 2014-06-24 15:25:02 +0800
comments: true
Category: Parse
Tags: Cache
Has_Math: True


### 討論
Parse 的 query 可以調整一些 cache policy，就是 可以決定在 query 時使用 cache 或是 network。各有優劣，以下略微說明：
<!--More-->

首先，舉個使用例子：

	:::Objective-C

    PFQuery *query = [PFQuery queryWithClassName:@"Station"];
    query.cachePolicy = kPFCachePolicyCacheThenNetwork;
    query.maxCacheAge = 60 * 60 * 24;

    [query findObjectsInBackgroundWithBlock:^(NSArray *objects, NSError *error) {
        if (!error) {
            for (PFObject *object in sortedObjects) {
                DMLog(@"%@", object[@"name"]);
            }
            self.stations = sortedObjects;
            [self.collectionView reloadData];
        } else {
            // Log details of the failure
            DMLog(@"Error: %@ %@", error, [error userInfo]);
        }

    }];


上面的 query 結果，會用一個 UITableView 列出來。重點是 `query.cachePolicy = kPFCachePolicyCacheThenNetwork` 告訴 Parse Framework 先查閱 cache，如果有，就先使用 cache，但是仍舊試圖用 network 查詢，若後端 server 有新資料，就會更新。因此 `findObjectsInBackgroundWithBlock` 後面的 block 會被執行兩次，首次執行沒有 cache 就會看到這樣：

	2014-06-24 17:05:37.335 ROD[31301:2340468] __36-[StationViewController viewDidLoad]_block_invoke Error: Error Domain=Parse Code=120 "The operation couldn’t be completed. (Parse error 120.)" UserInfo=0x112c90be0 {error=cache miss, code=120} {
	    code = 120;
	    error = "cache miss";
	}
	2014-06-24 17:05:37.620 ROD[31301:2340468] __36-[StationViewController viewDidLoad]_block_invoke xxx xxx xxx
	2014-06-24 17:05:37.620 ROD[31301:2340468] __36-[StationViewController viewDidLoad]_block_invoke xxx xxx xxx
	2014-06-24 17:05:37.620 ROD[31301:2340468] __36-[StationViewController viewDidLoad]_block_invoke xxx xxx xxx
	2014-06-24 17:05:37.620 ROD[31301:2340468] __36-[StationViewController viewDidLoad]_block_invoke xxx xxx xxx
	2014-06-24 17:05:37.620 ROD[31301:2340468] __36-[StationViewController viewDidLoad]_block_invoke xxx xxx xxx
	2014-06-24 17:05:37.620 ROD[31301:2340468] __36-[StationViewController viewDidLoad]_block_invoke xxx xxx xxx

第一次 block 執行會有 cache miss。第二次 block 執行，會到 network 查詢出來，然後 for loop 會 dump 查詢到的物件，本例子有 6 個。

之後如果再查詢，如果還在 cache age 有效期限內，將會看到一次是從 cache 查到，第二次是從 network 查到：

	2014-06-24 17:26:55.301 ROD[31301:2340468] __36-[StationViewController viewDidLoad]_block_invoke xxx xxx xxx
	2014-06-24 17:26:55.301 ROD[31301:2340468] __36-[StationViewController viewDidLoad]_block_invoke xxx xxx xxx
	2014-06-24 17:26:55.301 ROD[31301:2340468] __36-[StationViewController viewDidLoad]_block_invoke xxx xxx xxx
	2014-06-24 17:26:55.301 ROD[31301:2340468] __36-[StationViewController viewDidLoad]_block_invoke xxx xxx xxx
	2014-06-24 17:26:55.301 ROD[31301:2340468] __36-[StationViewController viewDidLoad]_block_invoke xxx xxx xxx
	2014-06-24 17:26:55.301 ROD[31301:2340468] __36-[StationViewController viewDidLoad]_block_invoke xxx xxx xxx

	2014-06-24 17:26:55.725 ROD[31301:2340468] __36-[StationViewController viewDidLoad]_block_invoke xxx xxx xxx
	2014-06-24 17:26:55.725 ROD[31301:2340468] __36-[StationViewController viewDidLoad]_block_invoke xxx xxx xxx
	2014-06-24 17:26:55.725 ROD[31301:2340468] __36-[StationViewController viewDidLoad]_block_invoke xxx xxx xxx
	2014-06-24 17:26:55.725 ROD[31301:2340468] __36-[StationViewController viewDidLoad]_block_invoke xxx xxx xxx
	2014-06-24 17:26:55.725 ROD[31301:2340468] __36-[StationViewController viewDidLoad]_block_invoke xxx xxx xxx
	2014-06-24 17:26:55.726 ROD[31301:2340468] __36-[StationViewController viewDidLoad]_block_invoke xxx xxx xxx


Parse Frame 提供的這個 cache policy 非常靈活好用。[這裡](https://parse.com/questions/cache-policy-help-and-best-practice)提供了關於 cache 的 Q&A。原始的 SDK Document 在[這裡](https://parse.com/docs/ios_guide#queries-caching/iOS)。
