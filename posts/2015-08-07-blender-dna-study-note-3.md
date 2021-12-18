layout: post
title: "Blender DNA Study Note 3"
date: 2015-08-07 17:36:48 +0800
comments: true
Category: Blender
Tags: Dna,Study
Has_Math: True

如同之前的 study, 這裏記錄一下 makesdna.c 的部分. 後文以 `blender root` 代表 blender source root.

這是一支獨立程式, 負責把 `blender root`/source/blender/makesdna/ 的 *.h 編碼成為一塊 data, 將來 append 在 .blend 最後面:

	00a74040: 0000 0000 0000 0000 0000 0000 0000 0000  ................
	00a74050: 0000 0000 0000 0000 0000 0000 0000 0000  ................
	00a74060: 0000 0000 0000 0000 0000 0000 0000 0000  ................
	00a74070: 0000 0000 0000 0000 0500 0000 0000 803f  ...............?
	00a74080: 0300 0000 0000 0000 0000 2041 0000 0000  .......... A....
	00a74090: 444e 4131 7450 0100 0826 ef18 0100 0000  DNA1tP...&......
	00a740a0: 0000 0000 0100 0000 5344 4e41 4e41 4d45  ........SDNANAME
	00a740b0: fe0f 0000 2a6e 6578 7400 2a70 7265 7600  ....*next.*prev.
	00a740c0: 2a64 6174 6100 2a66 6972 7374 002a 6c61  *data.*first.*la
	00a740d0: 7374 0078 0079 007a 0078 6d69 6e00 786d  st.x.y.z.xmin.xm
	00a740e0: 6178 0079 6d69 6e00 796d 6178 002a 706f  ax.ymin.ymax.*po

上面為任一個 .blend file 的 Hex Dump, 最左邊是位址, 中間是資料, 最右邊是對應的 ASCII.

可以看到檔案最後一段是 DNA1 開頭的 data block.

如果用 Xcode trace 的話, scheme 選擇 makesdna, `edit scheme | Run | Arguments` 手動輸入 __blender_dna.c /Users/tom/work/blender/source/blender/makesdna/__ 如下圖：


![](/images/makesdna xcode parameters.png)


產生後的 __blender_dna.c__ 會放在 `build-blender/bin/Debug/`, 其中, `build-blender` 是設定給 CMake 的 build directory.
