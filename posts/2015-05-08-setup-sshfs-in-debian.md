layout: post
title: "Setup SSHFS in Debian"
date: 2015-05-08 09:38:26 +0800
comments: true
Category: Linux
Tags:Fs
Has_Math: True

掛載遠端檔案系統，動作還算簡單，這裡有詳細的說明：

<http://blogger.gtwang.org/2014/05/sshfs-ssh-linux-windows-mac-os-x.html>
<!--MORE-->

先確認我們有安裝：

	sudo apt-get install sshfs

在 Debian 下有些要注意的事項：使用者必須加入 `fuse` group

	gpasswd -a USER_NAME fuse

其中 USER_NAME 是現在的帳號
__記得重新 login 以使生效__

之後才開始掛載動作：

	sshfs USER_NAME@REMOTE_HOST:/MOUNT_PATH LOCAL_MOUNT_PATH

其中 USER_NAME 是遠端帳號

REMOTE_HOST 是遠端主機

MOUNT_PATH 是遠端要被掛載的路徑

LOCAL_MOUNT_PATH 是近端機器的掛載點

#
