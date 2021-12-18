title: "Building Boost Lib"
date: 2018-01-16 20:56:00 +0800
Category: C++
Tags: Boost
Has_Math: True

Boost Lib 提供 C++ 開發者許多方便的工具, 從檔案系統工具到 thread programming,
從 data structures 到 algorithm,包山包海.

在 Mac OS 下如何安裝？我們可以視需要而定:

<H4>直接透過 brew</H4>

	$ brew install boost
	
就搞定了. Headers 會安裝在 /usr/local/include, Boost Library 會裝在 /usr/local/lib 下.

在 Debian/Linux 直接安裝:

	$ sudo apt install libboost-all-dev


<H4>從 source 來安裝</H4>

首先從 github 下載：

	$ git clone https://github.com/boostorg/boost.git
	
然後再下載 submodules:

	$ git submodule update --init --recursive

這裡相關 python 的部分, 內定使用 python 2, 以這裡的例子而言, 希望使用 python 3, 
	
	$ which python3
	$ /Library/Frameworks/Python.framework/Versions/3.6/bin/python3

下一步只有 Debian/Linux 要下命令, MacOS 跳過:
我們需要加裝一個套件, 以支援 unicode:

	$ sudo apt install libicu-dev
	
	
接下來 configure boost, 給定 --prefix=_[prefix-path]_ 將會使得 boost 把做好的 library 放在 _[prefix-path]_/lib 下, 
header files 放在 _[prefix-path]_/include 下:

	# set prefix-path to '/usr/local'
	$ ./boostrap.sh --prefix=/usr/local --with-python=python3 --with-icu

build it:

	$ ./b2
	
稍等片刻, build 完成後安裝它們：

	$ ./b2 install


安裝結果一樣, /usr/local/include 放 header files, /usr/local/lib 是 lib 的位置

#


