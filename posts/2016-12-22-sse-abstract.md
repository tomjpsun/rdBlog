title: "SSE 心得記錄"
date: 2016-12-22 16:21:22 +0800
Category: Misc
Tags: Cpu, Sse4
Has_Math: True

SSE 是從 Intel 的 x86 MMX 指令集擴充而來的. 

MMX 暫存器，稱作 MM0 到 MM7, 實際上就是處理器內部 80-bit 的浮點暫存器 st(0) 到 st(7)的尾數部分（64-bit）的復用。 SSE 將其擴充為 [128-bit](https://en.wikipedia.org/wiki/Streaming_SIMD_Extensions#Registers)

AMD 將 SSE2 更擴充為使用 0~15 個 128-bit 暫存器. Intel 接著也相容於此項設計.

SSE4 時代, 指令集更完備了, 方便應用在 Video, Image 相關領域.

128-bit 可以是 16 x 8-bit, 8 x 16-bit, 4 x 32-bit, 2 x 64-bit 的形式.

運算方式類似 SIMD: 一次平行計算這些資料, 細節可以看下面參考資料.

怎樣在程式碼使用呢？一般都是用一個 __m128 struct 來代表一個 SSE unit (即對應一個 register)

	:::c
	typedef float __m128 __attribute__((__vector_size__(16)));

又, 因為方便 CPU cache 存取的關係, 我們會希望資料為 16-byte alignment:

	:::c
	__m128 x __attribute__ ((aligned (16)));

應用部分, [Intel SSE 的參考資料]((https://software.intel.com/sites/default/files/m/a/9/b/7/b/1000-SSE.pdf)) 介紹得很好,
有興趣的可以接下去看.

基本的使用方面, hotball 的介紹很清楚,現在的 gcc preprocessing 或 llvm clang 都有支援 sse intrinsic, 所以很容易使用 #

參考資料：

[Intel SSE slider](https://software.intel.com/sites/default/files/m/a/9/b/7/b/1000-SSE.pdf)

[SSE 介紹](https://www.csie.ntu.edu.tw/~r89004/hive/sse/page_1.html)
