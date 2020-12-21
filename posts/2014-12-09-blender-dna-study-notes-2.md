layout: post
title: "Blender DNA Study Notes 2"
date: 2014-12-09 10:29:45 +0800
comments: true
Category: Blender
Tags: Dna, Study
Has_Math: True

Blender 開始時，會呼叫 `DNA_sdna_from_data()` 讀取內建於 memory 的 DNA 字串：`DNAstr`
然後輾轉呼叫到 `init_structDNA(SDNA *sdna, bool do_endian_swap)`，這是最底層的檔案 parsing 動作，傳入的 `sdna->data` 已經 copy 了 `DNAstr` 的內容，這個 function 會 parsing 這塊 data，取出的值填入 `SDNA *sdna`
<!--More-->
先看 DNAstr 的開頭內容:

	(lldb) memory read --count 64 data
	0x10d0c5e08: 53 44 4e 41 4e 41 4d 45 69 0f 00 00 2a 6e 65 78  SDNANAMEi...*nex
	0x10d0c5e18: 74 00 2a 70 72 65 76 00 2a 64 61 74 61 00 2a 66  t.*prev.*data.*f
	0x10d0c5e28: 69 72 73 74 00 2a 6c 61 73 74 00 78 00 79 00 7a  irst.*last.x.y.z
	0x10d0c5e38: 00 78 6d 69 6e 00 78 6d 61 78 00 79 6d 69 6e 00  .xmin.xmax.ymin.
	(lldb)

可以看到檔頭 `SDNA`，接著第一個 file block `NAME`，接著 number of names (69 0f 00 00 = 0xf69)，然後就是每一個 name string

接下來看`init_structDNA()`如何 parsing 這一塊：

	:::c++

	static void init_structDNA(SDNA *sdna, bool do_endian_swap)
	{
		int *data, *verg, gravity_fix = -1;
		intptr_t nr;
		short *sp;
		char str[8], *cp;

		verg = (int *)str;
		data = (int *)sdna->data;

		strcpy(str, "SDNA");
		if (*data == *verg) {

			data++;

			/* load names array */
			strcpy(str, "NAME");
			if (*data == *verg) {
				data++;

				if (do_endian_swap) sdna->nr_names = le_int(*data);
				else sdna->nr_names = *data;

				data++;
				sdna->names = MEM_callocN(sizeof(void *) * sdna->nr_names, "sdnanames");
			}
			else {
				printf("NAME error in SDNA file\n");
				return;
			}

			nr = 0;
			cp = (char *)data;
			while (nr < sdna->nr_names) {
				sdna->names[nr] = cp;

				/* "float gravity [3]" was parsed wrong giving both "gravity" and
				 * "[3]"  members. we rename "[3]", and later set the type of
				 * "gravity" to "void" so the offsets work out correct */
				if (*cp == '[' && strcmp(cp, "[3]") == 0) {
					if (nr && strcmp(sdna->names[nr - 1], "Cvi") == 0) {
						sdna->names[nr] = "gravity[3]";
						gravity_fix = nr;
					}
				}

				while (*cp) cp++;
				cp++;
				nr++;
			}



比對檔頭之後，取得 number of names 填入 `sdna->nr_names`，`le_int()` 會做 endian 的轉換，使得 blender file 在 Big Endian 與 Little Endian 系統下都可讀。


line 34,35 是依序把 `sdna->names[]` 指到個別的 names 開頭

line 40-44 是處理某個特例，在此省略

line 47 是滑過一個字串

line 48 是跳過字串結尾，指向下一個字串開頭

line 49 nr 是陣列的索引

__因為 sdna->data 是 copy 來的，這段 memory 會一直保存住__，所以 `sdna->names[]` 只要指標指到即可

至此, 我們就把 sdna->names 填好了, 裡面指到 data 所有的 name:

![initSDNANames.png](http://coding-addict.com/pictures/rd/initSDNANames.png)

接下來處理 type names 與 type lengths，程式邏輯類似，我們就略過

最後來到比較複雜的 struct parsing:

	:::c++

		/* load struct array */
		data = (int *)sp;
		strcpy(str, "STRC");
		if (*data == *verg) {
			data++;

			if (do_endian_swap) sdna->nr_structs = le_int(*data);
			else sdna->nr_structs = *data;

			data++;
			sdna->structs = MEM_callocN(sizeof(void *) * sdna->nr_structs, "sdnastrcs");
		}
		else {
			printf("STRC error in SDNA file\n");
			return;
		}

		nr = 0;
		sp = (short *)data;
		while (nr < sdna->nr_structs) {
			sdna->structs[nr] = sp;

			if (do_endian_swap) {
				short a;

				sp[0] = le_short(sp[0]);
				sp[1] = le_short(sp[1]);

				a = sp[1];
				sp += 2;
				while (a--) {
					sp[0] = le_short(sp[0]);
					sp[1] = le_short(sp[1]);
					sp += 2;
				}
			}
			else {
				sp += 2 * sp[1] + 2;
			}

			nr++;
		}


參考下面圖示，這一段的動作，主要是 line 21 : 建立由 `sdna->structs` 到 data 的指標

![initStructDNA](http://coding-addict.com/pictures/rd/initStructDNA.png)

最後一段，

	:::c++

	/* finally pointerlen: use struct ListBase to test it, never change the size of it! */
			sp = findstruct_name(sdna, "ListBase");
			/* weird; i have no memory of that... I think I used sizeof(void *) before... (ton) */

			sdna->pointerlen = sdna->typelens[sp[0]] / 2;

			if (sp[1] != 2 || (sdna->pointerlen != 4 && sdna->pointerlen != 8)) {
				printf("ListBase struct error! Needs it to calculate pointerize.\n");
				exit(1);
				/* well, at least sizeof(ListBase) is error proof! (ton) */
			}

			/* second part of gravity problem, setting "gravity" type to void */
			if (gravity_fix > -1) {
				for (nr = 0; nr < sdna->nr_structs; nr++) {
					sp = sdna->structs[nr];
					if (strcmp(sdna->types[sp[0]], "ClothSimSettings") == 0)
						sp[10] = SDNA_TYPE_VOID;
				}
			}

	#ifdef WITH_DNA_GHASH
			/* create a ghash lookup to speed up */
			sdna->structs_map = BLI_ghash_str_new_ex("init_structDNA gh", sdna->nr_structs);

			for (nr = 0; nr < sdna->nr_structs; nr++) {
				sp = sdna->structs[nr];
				BLI_ghash_insert(sdna->structs_map, (void *)sdna->types[sp[0]], (void *)(nr + 1));
			}
	#endif
		}
	}



因為 ListBase 有兩個指摽：

	:::c++

	typedef struct ListBase  {
		void *first, *last;
	} ListBase;


所以這裏用 ListBase struct type length 除以 2，得到一個指標的 size，填入 `sdna->pointerlen`

line 7 的邏輯判斷，後面是廢句不會生效，`sp[0]`是 ListBase 的 type 編碼，`sp[1]`是 ListBase struct 的成員個數，應該有兩個，不對就報錯！

然後對於宣告`gravity`處理上有問題，所以另外 patch line 14-20，暫時看不懂，不理會這一段。

最後是建立 hash map，先看一下 GHash 宣告：

	:::c++

	struct GHash {
		GHashHashFP hashfp;
		GHashCmpFP cmpfp;

		Entry **buckets;
		struct BLI_mempool *entrypool;
		unsigned int nbuckets;
		unsigned int nentries;
		unsigned int cursize, flag;
	};


這裏 hashfp 是 hash 計算函數，cmpfp 是 hash key compare 函數， hash 的大小由一張 table 決定：

	:::c++

	const unsigned int hashsizes[] = {
		5, 11, 17, 37, 67, 131, 257, 521, 1031, 2053, 4099, 8209,
		16411, 32771, 65537, 131101, 262147, 524309, 1048583, 2097169,
		4194319, 8388617, 16777259, 33554467, 67108879, 134217757,
		268435459
	};


hash 隨著使用需要可以增加 buckets，每增加一級，對應的 hash size， 就靠這個陣列取得
下面為新增 hash 的函數：

	:::c++

	static GHash *ghash_new(GHashHashFP hashfp, GHashCmpFP cmpfp, const char *info,
	                        const unsigned int nentries_reserve,
	                        const unsigned int entry_size)
	{
		GHash *gh = MEM_mallocN(sizeof(*gh), info);

		gh->hashfp = hashfp;
		gh->cmpfp = cmpfp;

		gh->nbuckets = hashsizes[0];  /* gh->cursize */
		gh->nentries = 0;
		gh->cursize = 0;
		gh->flag = 0;

		/* if we have reserved the number of elements that this hash will contain */
		if (nentries_reserve) {
			ghash_buckets_reserve(gh, nentries_reserve);
		}

		gh->buckets = MEM_callocN(gh->nbuckets * sizeof(*gh->buckets), "buckets");
		gh->entrypool = BLI_mempool_create(entry_size, 64, 64, BLI_MEMPOOL_NOP);

		return gh;
	}


 初始化 struct GHash 各個欄位，`nbuckets` 預設使用第一級 hashsizes，然後在 `ghash_buckets_reserve()` 調整到需要的 size，`entrypool`是當作小型的 bucket pool，只要需要 bucket 時就跟它調用

#




## 參考資料 ##
[Blender File Format](http://www.atmind.nl/blender/mystery_ot_blend.html) 提供有關 Blender File 詳細的說明
