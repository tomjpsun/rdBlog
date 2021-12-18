layout: post
title: "Build Bluez Under Debian"
date: 2015-05-18 17:03:21 +0800
comments: true
Category: Linux
Tags: Bluetooth
Has_Math: True

這裏介紹如何在 Debian Wheezy 下編譯 Bluez 5.30：
<!--More-->

	git clone git://git.kernel.org/pub/scm/bluetooth/bluez.git

如果沒有 ./configure 就從 ./bootstrap-configure 開始，
要加一些參數，否則會遇到

	configure: error: systemd system unit directory is required

命令為：

	./bootstrap-configure
	./configure --with-systemdsystemunitdir=/lib/systemd/system --with-systemduserunitdir=/usr/lib/systemd

