title: Setup Cereal Package
slug: setup_cereal
date: 2021-02-05 12:33:45 UTC+08:00
type: text

Cereal 套件是將程式結果序列儲存到檔案的工具, 大概原本 boost lib 太肥大了,
所以出現這個套件, 它是 C++ Header Only, 因為直接下 Github source 安裝,
我在過程中撞到牆, 特此記錄.

一開始, 直接用 make install, 會發現一堆無法找到的 headers, 如 <gnu/stubs-32.h>
等等, 這個專案已經進入蠻穩定的階段, 不太合理, 因為.... 就不是這樣安裝啊！

正確 install 如下:

	# github 下載後
	cd cereal && git checkout develop
	mkdir build && cd build && cmake .. -DJUST_INSTALL_CEREAL=ON
	sudo make install

這樣就直接裝入系統 include path 啦！

因為 mlpack 現在在 cmake 時, 會檢查 Cereal, 最低版本需求 Debian 還沒有打包,
因此需要自己安裝！

#
