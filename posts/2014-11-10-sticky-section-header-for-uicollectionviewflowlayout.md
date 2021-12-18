layout: post
title: "Sticky Section Header for UICollectionViewFlowLayout"
date: 2014-11-10 21:38:45 +0800
comments: true
Category: Ios
Tags: Uicollectionviewflowlayout, Cocoapods
Has_Math: True

我們在使用 UICollectionView 的時候，有時希望特製一些 Cell 特別的排列動作，CocoaTouch Framework 有提供 UICollectionViewFlowLayout 這個 class，提供基本的流水排列方式，可以是 Virtical 或 Horizontal 的流水型的排列。

<!--More-->

關於 UICollectionViewFlowLayout 的特製方式，Apple 網站有提供[CollectionViewTransition Sample](https://developer.apple.com/library/ios/samplecode/CollectionViewTransition/Introduction/Intro.html)
重點之一是繼承後，overwrite `layoutAttributesForElementsInRect:`，然後在裡面設定 cell 的 attributes，包括 frame, center, transition 等等。

GitHub 上也有一個寫得很好的 sample : [CSStickyHeaderFlowLayout](https://github.com/jamztang/CSStickyHeaderFlowLayout)，本篇 blog 就是解釋此專案的做法。

首先對一些東西圖示，等下搭配 code 來看，就會很方便了。

![](/images/UICollectionViewFlowLayout code study.png)

* 左圖標示出畫面有哪些 Sections & Cells，parallax header 定義為 `NSString *const CSStickyHeaderParallaxHeader = @"CSStickyHeaderParallexHeader";`
* 中間圖標示出程式裏的 parallax header 的 reference size 與 minimum reference size
* 右圖標示出當往上捲動時，Section header 是否會 __停滯__ 在最上面 （直到該 section 捲完才換下一個 section header __停滯__ ，這個特性由 disableStickyHeaders 控制。

下面列出這個專案中，最重要的程式片段與註解：

	:::Objective-C

	- (NSArray *)layoutAttributesForElementsInRect:(CGRect)rect
	{
	    /* 調整 rect 讓 我們自訂的 parallax header 可以被加進來，
	     super 會依照調整過的 rect 幫我們抓 rect 內的 section headers & cells
	     */
	    CGRect adjustedRect = rect;
	    adjustedRect.origin.y -= self.parallaxHeaderReferenceSize.height;
	    NSMutableArray *allItems
	    = [[super layoutAttributesForElementsInRect:adjustedRect] mutableCopy];

	    /*現在檢視 allItems 裏的每一個項目，並且令
	        headers 字典紀錄遇到的 section header，並且以它們對應到的 section number 當作 key，
	        lastCells 字典紀錄每個 section 最後一個 cell，也是以 section number 當作 key，
	        visibleParallexHeader 紀錄現在是否可以看到 我們自訂的 parallex header。
	     */
	    NSMutableDictionary *headers = [[NSMutableDictionary alloc] init];
	    NSMutableDictionary *lastCells = [[NSMutableDictionary alloc] init];
	    __block BOOL visibleParallexHeader;

	    [allItems enumerateObjectsUsingBlock:^(id obj, NSUInteger idx, BOOL *stop) {
	        UICollectionViewLayoutAttributes *attributes = obj;

	        /* 這裏有點特別：無條件把每一個 frame 向下移，空出空間放 parallax header
	            若沒有這三行，parallax header 會被 cell / section header 蓋掉
	         */
	        CGRect frame = attributes.frame;
	        frame.origin.y += self.parallaxHeaderReferenceSize.height;
	        attributes.frame = frame;

	        /* 前面提過，要將所有 attributes 做分類，並分別紀錄到個別的字典，這裡就是了
	         */
	        NSIndexPath *indexPath = [(UICollectionViewLayoutAttributes *)obj indexPath];
	        if ([[obj representedElementKind]
	        		isEqualToString:UICollectionElementKindSectionHeader]) {
	            [headers setObject:obj forKey:@(indexPath.section)];
	        } else if ([[obj representedElementKind]
	        		isEqualToString:UICollectionElementKindSectionFooter]) {
	            // Not implemeneted
	        } else {
	            NSIndexPath *indexPath = [(UICollectionViewLayoutAttributes *)obj indexPath];

	            UICollectionViewLayoutAttributes *currentAttribute =
	            	[lastCells objectForKey:@(indexPath.section)];

	            /* 紀錄 section 的最後一個 cell，若找到 cell 編號比現在更後面的，就把他紀起來，將原本所紀錄的 替換掉
	             */
	            if ( ! currentAttribute || indexPath.row > currentAttribute.indexPath.row) {
	                [lastCells setObject:obj forKey:@(indexPath.section)];
	            }

	            /* 如果現在可看到 cell indexPath = (0,0) 表示 parallax header 應該也看得到
	             */
	            if ([indexPath item] == 0 && [indexPath section] == 0) {
	                visibleParallexHeader = YES;
	            }
	        }

	        // For iOS 7.0, the cell zIndex should be above sticky section header
	        attributes.zIndex = 1;
	    }];

	    // when the visible rect is at top of the screen, make sure we see
	    // the parallex header
	    if (CGRectGetMinY(rect) <= 0) {
	        visibleParallexHeader = YES;
	    }

	    if (self.parallaxHeaderAlwaysOnTop == YES) {
	        visibleParallexHeader = YES;
	    }


	    // This method may not be explicitly defined, default to 1
	    // https://developer.apple.com/library/ios/documentation/uikit/reference/UICollectionViewDataSource_protocol/Reference/Reference.html#jumpTo_6
	    NSUInteger numberOfSections = [self.collectionView.dataSource
	                                   respondsToSelector:@selector(
	                                   numberOfSectionsInCollectionView:)]
	                                ? [self.collectionView.dataSource
	                                 numberOfSectionsInCollectionView:self.collectionView]
	                                : 1;

	    /* 這段表示要處理 parallax header --- 判斷需不需要處理 parallax header 的條件是 :
	     1.現在的 rect 看得到 parallax header
	     2. Parallax header size 不為零
	     */
	    if (visibleParallexHeader &&
	    ! CGSizeEqualToSize(CGSizeZero, self.parallaxHeaderReferenceSize)) {

	        /*
	         我們宣告 parallex header 的種類為自製的 supplementary view (即 CSStickyHeaderParallaxHeader)，
	         這有別於系統內建的 UICollectionElementKindSectionHeader，我們在這裡製造一個 attribute 給 parallax header

	         其中 self.parallaxHeaderReferenceSize 是在 collection view viewDidLoad: 指定的，如圖標示
	         */
	        CSStickyHeaderFlowLayoutAttributes *currentAttribute =
	        		 [CSStickyHeaderFlowLayoutAttributes
	        		 layoutAttributesForSupplementaryViewOfKind:CSStickyHeaderParallaxHeader
	        		 withIndexPath:[NSIndexPath indexPathWithIndex:0]];

	        /* 以下開始設定 parallax header 的 attributes : 首先設定 frame
	         */
	        CGRect frame = currentAttribute.frame;
	        frame.size.width = self.parallaxHeaderReferenceSize.width;
	        frame.size.height = self.parallaxHeaderReferenceSize.height;

	        CGRect bounds = self.collectionView.bounds;
	        CGFloat maxY = CGRectGetMaxY(frame);

	        /* MIN 裡面第一項 表示 允許被蓋掉的部分，意即圖示中 parallax header 的高度
	         扣除 parallax header minimum 的高度 的部分。
	         MIN 第二項 表示 collection view 已經捲動的高度（如果有 navigation bar 的時候，
	         要把 navigation bar 的高度也算入（就是 contentInset）

	         這個 MIN 就是當捲動高度不大於 允許被蓋掉的高度 時，得到捲動的高度。
	         當大於 允許被蓋掉的高度時，就都只給出 允許被蓋掉的高度，它是一個常數值
	         (因為 maxY & minimum reference size height 都是定值)。
	         */
	        CGFloat y = MIN(maxY - self.parallaxHeaderMinimumReferenceSize.height,
	        bounds.origin.y + self.collectionView.contentInset.top);

	        /* 算出 parallax header 高度減掉捲動的高度，
	         一開始為 parallax header reference height，隨著向上滑動而減少值，最小為
	         parallax header minimum reference height，這個 MAX 只是保證計算所得一定要大於 0
	         */
	        CGFloat height = MAX(0, -y + maxY);

	        /* 算出目前 parallax header 被蓋掉的比率，本模組其他部分都沒有使用，僅供參考
	         */
	        CGFloat maxHeight = self.parallaxHeaderReferenceSize.height;
	        CGFloat minHeight = self.parallaxHeaderMinimumReferenceSize.height;
	        CGFloat progressiveness = (height - minHeight)/(maxHeight - minHeight);
	        currentAttribute.progressiveness = progressiveness;

	        /* 因為所有的 cell zIndex = 1， 設定 parallax header zIndex = 0 可以確保
	         cell 會覆蓋 parallax header
	         */
	        currentAttribute.zIndex = 0;

	        // When parallaxHeaderAlwaysOnTop is enabled, we will check when we should update the y position
	        if (self.parallaxHeaderAlwaysOnTop &&
	        height <= self.parallaxHeaderMinimumReferenceSize.height) {
	            CGFloat insetTop = self.collectionView.contentInset.top;
	            // Always stick to top but under the nav bar
	            y = self.collectionView.contentOffset.y + insetTop;
	            currentAttribute.zIndex = 2000;
	        }

	        currentAttribute.frame = (CGRect){
	            frame.origin.x,
	            y,
	            frame.size.width,
	            height,
	        };


	        [allItems addObject:currentAttribute];
	    }

	    /* 如果有設定 sticky header 的話，讓每一個 section header 都 sticky on top，直到它的 section 裏最後一個 cell 離開，才離開。
	     */
	    if ( ! self.disableStickyHeaders) {
	        [lastCells enumerateKeysAndObjectsUsingBlock:^(id key, id obj, BOOL *stop) {
	            NSIndexPath *indexPath = [obj indexPath];
	            NSNumber *indexPathKey = @(indexPath.section);

	            UICollectionViewLayoutAttributes *header = headers[indexPathKey];
	            // CollectionView automatically removes headers not in bounds
	            if ( ! header) {
	                header = [self
	                layoutAttributesForSupplementaryViewOfKind: UICollectionElementKindSectionHeader
	                 atIndexPath:[NSIndexPath indexPathForItem:0 inSection:indexPath.section]];

	                if (header) {
	                    [allItems addObject:header];
	                }
	            }
	            [self updateHeaderAttributes:header
	            lastCellAttributes:lastCells[indexPathKey] ];
	        }];
	    }

	    return allItems;
	}


[updateHeaderAttributes:....] 參數輸入 section header attribute 以及該 section 最後一個 cell 的 attribute，根據這兩個參數算出 section header 的 y 位置值，使得 section header 可以 `stick` 在同一個位置直到該 section 結束。

	:::Objective-C

	- (void)updateHeaderAttributes:(UICollectionViewLayoutAttributes *)attributes
	lastCellAttributes:(UICollectionViewLayoutAttributes *)lastCellAttributes
	{
	    CGRect currentBounds = self.collectionView.bounds;
	    attributes.zIndex = 1024;
	    attributes.hidden = NO;

	    CGPoint origin = attributes.frame.origin;

	    CGFloat sectionMaxY = CGRectGetMaxY(lastCellAttributes.frame) -
	    		attributes.frame.size.height;
	    CGFloat y = CGRectGetMaxY(currentBounds) -
	    		currentBounds.size.height +
	    		self.collectionView.contentInset.top;

	    if (self.parallaxHeaderAlwaysOnTop) {
	        y += self.parallaxHeaderMinimumReferenceSize.height;
	    }

	    CGFloat maxY = MIN(MAX(y, attributes.frame.origin.y), sectionMaxY);

	    //NSLog(@"y = %.2f, maxY = %.2f, sectionMaxY = %.2f", y, maxY, sectionMaxY);

	    origin.y = maxY;

	    attributes.frame = (CGRect){
	        origin,
	        attributes.frame.size
	    };
	}


小結：CSStickyHeaderFlowLayout 這個 Cocoapod 把複雜的動作封裝在 UICollectionViewFlowLayout 衍生的 class 中，目前這種折疊式的呈現方式，是很流行的 UI 設計，本專案以 MIT License 提供給大家使用！
