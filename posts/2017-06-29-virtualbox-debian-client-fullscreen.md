title: "Virtualbox with Debian 9 Client Setup"
date: 2017-06-29 22:30:00 +0800
Category: Linux
Tags: Vbox, Virtualbox, Addon, Debian 9
Has_Math: True

每次在 VirtualBox 裡頭安裝 Debian client 都要面對如何安裝 vbox addon 的問題.

在 Debian 9 已經把 virtualbox 相關的 packages 移到另外的 repository: Oracle 的 site,
下面說明如何安裝, 本文參考 [rationallyPARANOID](https://rationallyparanoid.com/articles/debian-9-virtualization.html) 的文件.
該文雖然是以 Debian 9 為 host machine, 但是取得 pgp key 的步驟是一樣的, 也就是說：我們將在 Debian 9 client 裡面做同樣的動作.

下載 Oracle pgp key:

	$ wget -S https://www.virtualbox.org/download/oracle_vbox_2016.asc

在 root 權限下安裝 key:

	# apt-key add oracle_vbox_2016.asc

check it:

	# apt-key list
	.....
	pub   rsa4096 2016-04-22 [SC]B9F8 D658 297A F3EF C18D  5CDF A2F6 83C5 2980 AECF
	uid           [ unknown] Oracle Corporation (VirtualBox archive signing key) <info@virtualbox.org>
	sub   rsa4096 2016-04-22 [E]
	.....

setup apt source list:

	# echo "deb http://download.virtualbox.org/virtualbox/debian stretch contrib" >> /etc/apt/sources.list.d/virtualbox.list
	
現在可以更新與安裝了:

	# apt-get update
	# apt-get install virtualbox-5.1

到這裡, Debian client 所需的 packages 已經準備好了.
接下來要把 Virtualbox addon 裝起來

到 Virtualbox 網站下載 VirtualBox _[version xxx]_ Oracle VM VirtualBox Extension Pack

![](/images/setup-virtualbox-guest-addition-iso.png)

在 Debian 9 client 執行時, 把它 mount 起來, 應該會在 /media/cdrom 下面看到它的內容：

	tom@debian9:~$ ls /media/cdrom
	32Bit        autorun.sh  runasroot.sh              VBoxWindowsAdditions-amd64.exe
	64Bit        cert        VBoxLinuxAdditions.run    VBoxWindowsAdditions.exe
	AUTORUN.INF  OS2         VBoxSolarisAdditions.pkg  VBoxWindowsAdditions-x86.exe

用 root 權限執行 VBoxLinuxAdditions.run 即可完成！

	# sudo bash ./VBoxLinuxAdditions.run
#
