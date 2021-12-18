title: "Setup vnc server on Debian Stretch (Cinnamon WM)"
date: 2018-02-03 23:56:00 +0800
Category: Setup
Tags: Debian, System Configure, Vnc
Has_Math: True

在 Debian Stretch 架設 vnc server, 都可能在連上後, 遇到這樣的問題：
# “Could not acquire name on session bus” #

<!-- more -->

我是 Debian 9.0 (stretch), Window Manager 採用 Cinnamon, vnc 是 `tigervnc-standalone-server` 套件.
(p.s. [這裡](https://forums.linuxmint.com/viewtopic.php?t=165301)提到 Cinnamon 搭配 tigervnc 沒問題, 其他 vnc server 有問題)


所有的網路解法都說要把 unset DBUS_SESSION_BUS_ADDRESS 加到設定檔案: .vnc/xstart, 例如:

[http://blog.csdn.net/dengruijin/article/details/46575681](http://blog.csdn.net/dengruijin/article/details/46575681)

但是我還有另一個問題---啟動的權限沒有設定好：
	
	(x)
	$ vncserver -depth 24 -geometry 1280x1024
		
應該加上 -localhost no 才不會忽略 remote connection：

	(o)
	$ vncserver -localhost no -depth 24 -geometry 1280x1024

剩下就是 .vnc/xstart 設定檔案了, 這樣就可啟動 cinnamon session:

	#!/bin/sh

	unset SESSION_MANAGER
	unset DBUS_SESSION_BUS_ADDRESS
	[ -x /etc/vnc/xstartup ] && exec /etc/vnc/xstartup
	[ -r $HOME/.Xresources ] && xrdb $HOME/.Xresources
	xsetroot -solid grey
	vncconfig -iconic &
	x-terminal-emulator -geometry 80x24+10+10 -ls -title "$VNCDESKTOP Desktop" &
	x-window-manager &
	# wm-session &
	export STARTUP="/usr/bin/cinnamon-session"
	$STARTUP

#
