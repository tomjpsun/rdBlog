title: "Build GNUstep environment"
date: 2013-09-01 21:31
comments: true
Category: Gnustep
Tags: Build
Has_Math: True

花了兩天時間弄好，其實建立環境的步驟，不難。但是遇到問題要解，就會費時間了。

基本步驟可以分為幾個：
<!-- more -->
* Install prerequisite packages
* Build LLVM + Clang
* Build libdispatch
* Build libojbc
* Build GNUstep
components under gnustep/core/
`make`
`base`
`gui `
`back`

Host : Debian (arch `amd64`)

[Note 1] 在 `i386` 遇到 clang-3.2 的路徑問題，要等 clang-3.2~exp5 才被 fix， 但是我是 clang-3.4 仍然遇到同樣現象，只好換成 `amd64` 當作 Host.

Path detection issue under `i386` :
<http://bugs.debian.org/cgi-bin/bugreport.cgi?bug=697127>

為何要用 clang 而不是 gcc ?



##Install prerequisite packages##
Reference the first part of [script 1]，僅僅把前三個 sudo aptitude install … 執行一次即可。

	# Dependencies
	sudo aptitude install build-essential git subversion ninja cmake
	# Dependencies for GNUStep Base
	sudo aptitude install libffi-dev libxml2-dev libgnutls-dev libicu-dev
	# Dependencies for libdispatch
	sudo aptitude install libblocksruntime-dev libkqueue-dev libpthread-workqueue-dev autoconf libtool

[script 1] <http://wiki.gnustep.org/index.php/GNUstep_under_Ubuntu_Linux>

##Build LLVM + Clang##
依照官方標準步驟 <http://clang.llvm.org/get_started.html>

svn 很久， build 也很久。這裡提供一個快速回復原始下載的指令，in package `subversion-tools`：

	sudo aptitude install subversion-tools
	svn-clean <your svn src dir>

[Note 2]
configure for Release llvm+clang

	mkdir build
	cd build
	../llvm/configure --enable-optimized

	The generated executables are located in build/Release+Asserts/bin/

##Build libobjc##
依照 [scrip 1] 裡面關於 libobjc 的部份，當然要使用 clang.

	clang -v
	clang++ -v

	cd ~/libobjc2
	mkdir build
	cd build
	cmake ..
	make -j8
	sudo -E make install

[script 1] <http://wiki.gnustep.org/index.php/GNUstep_under_Ubuntu_Linux>


這裡遇到雷(多到我無法理解原因，只能記下最後解法)，後來移掉 g++ & gobjc 兩個 packages 得以解決。
<http://stackoverflow.com/questions/14980402/how-to-make-gnustep-libobjc2-to-work>

##Build GNUStep##
依照 [script 2] Line 33 開始的步驟即可（前面的 clang 與 svn 擷取，要自行 mark 掉）

[script 2]
<https://bitbucket.org/ivucica/gnustep-ubuntu/src/b8d7612aadc3/GNUstep%20libobjc2%20on%20Ubuntu.sh>

##結語##
* 為了解決問題，使用重新安裝的 Debian (amd64)，但是還是遇到了許多 libobjc & GNUstep 的 build 問題。所以不是安裝環境凌亂的因素；依照這裡的 [script 1] 仍無法順利進行.

* 上面 [script 1] 與 [script 2] 皆無法順利一口氣 build 出來，因為沒有很多時間去追究問題點，這次筆記，只是大約的記下這兩天的解決過程。

* 本計畫在 kickstarter 募資了。目的是希望投入更多人力，作跨平台的開發。但是我經過這兩天的挫折，覺得計畫開發者，可能應該先花些功夫，讓基礎環境建置，更容易一些才對！
<http://gnustep.8.n7.nabble.com/GNUstep-Kickstarter-Project-td34670.html>
