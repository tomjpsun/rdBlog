layout: post
title: "Build OpenCV on OS X El Capitan"
date: 2016-05-10 09:52:39 +0800
comments: true
Category: Opencv
Tags:Build
Has_Math: True

OpenCV 是一套 image processing library, 網路上的教學很多, 我們在這裡就來分享如何 __build from source__.
<!--More-->
本文的主要參考文件是：

[HOWTO: Install, Build and Use openCV (MacOSX 10.10)](http://blogs.wcode.org/2014/10/howto-install-build-and-use-opencv-macosx-10-10/)

__Build 所需環境：__

- Xcode 7.3.1

- CUDA 7.5.27, 不然會有 nvcc 的問題: `nvcc fatal : The version ('70300') of the host compiler ('Apple clang') is not supported` (遇到這個問題，可能要退回 更早的 OS 版本才能 build)

- CMake, 我裝 3.3.2 OK

__Build Steps:__

OpenCV configuration 可以提供 static 或 shared build, 我選擇 build shared lib, 所以抄一下[剛才參考的文件](http://blogs.wcode.org/2014/10/howto-install-build-and-use-opencv-macosx-10-10/)：

打開 CMake, 自建一個 SharedLib 目錄當做 __Where to build the binaries 欄位__, 然後設定下列 configurations：

    Check BUILD_SHARED_LIBS

    Uncheck BUILD_TESTS

    Add an SDK path to CMAKE_OSX_SYSROOT, it will
    look something like this：
    “/Applications/Xcode.app/Contents/Developer/
    Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.9.sdk”.

    Add x86_64 to CMAKE_OSX_ARCHITECTURES,
    this tells it to compile against the current system

    Uncheck WITH_1394

    Uncheck WITH_FFMPEG

然後進入 SharedLib, 開始 build

	make
	make install

如果順利, 在 /usr/local/lib/ 下會看到一堆 libopencv_*.dylib

__怎樣使用?__

用文字編輯器, 編輯下列測試程式： test.cpp

	:::c

	#include "opencv2/imgproc/imgproc.hpp"
	#include "opencv2/highgui/highgui.hpp"

	using namespace std;
	using namespace cv;

	Mat src; Mat dst;
	char window_name1[] = "Unprocessed Image";
	char window_name2[] = "Processed Image";

	int main( int argc, char** argv )
	{
	    /// Load the source image
	    src = imread( argv[1], 1 );

	    namedWindow( window_name1, WINDOW_AUTOSIZE );
	    imshow("Unprocessed Image",src);

	    dst = src.clone();
	    GaussianBlur( src, dst, Size( 15, 15 ), 0, 0 );

	    namedWindow( window_name2, WINDOW_AUTOSIZE );
	    imshow("Processed Image",dst);

	    waitKey();
	    return 0;
	}



這測試會把圖檔做高斯處理。

編譯指令：

	g++ $(pkg-config --cflags --libs opencv) test.cpp -o ocvTest


準備一張圖 `sample.jpg` 放在同目錄下當做 source.

執行：

	./ocvTest sample.jpg


可以看到結果：
<img src="http://coding-addict.com/pictures/rd/opencv_test.png" alt="Drawing" style="width: 300px;"/>

#
