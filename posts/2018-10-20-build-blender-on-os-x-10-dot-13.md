title: "Build Blender 2.8"
date: 2018-10-20 18:00:00 +0800
Category: C++
Tags: Build
Has_Math: True

This description here is valid in OS X 10.13 (High Sierra) / Xcode 10.0 / CMake 3.10.

Currently, the Blender source has been to 2.79/2.8 beta

<!-- more -->

##On Mac High Sierra

主要參考文件：[Building Blender/Mac](https://wiki.blender.org/wiki/Building_Blender/Mac)

時隔三年, [舊文](http://rd.coding-addict.com/build-blender-on-os-x-1010.html)  的環境已經不同,
所以再此重寫一篇.

要先準備 Xcode 10.0 以及 Xcode 10.0 Development Tools.
依照剛剛的 link 的指示, 順便設定 CMake command line.

下載 Blender source:

	mkdir ~/blender-dev
	cd ~/blender-dev
	git clone http://git.blender.org/blender.git
	cd blender
	make update

當然, cd `blender` 之後若要 build Blender 2.8, 就要用 git 切換到 branch `blender2.8`,
build 步驟都跟 master branch 相同.

然後回到我們的 `~/blender-dev` 目錄, 我們將在這裡建立一個 `build-blender` 目錄:

	cd ~/blender-dev
	mkdir build-blender
	cd build-blender
	cmake -DCMAKE_EXPORT_COMPILE_COMMANDS=1 ../blender
	make

產生的 app 會在 `~/blender-dev/build-blender/bin` 目錄.
#
