Title: "Virtual Machine Manager Bridge Setup"
date: 2020-05-20 08:01:00 +0800
Category: Virtualization
Tags: Vmm Bridge
Has_Math: True

自從 virtualization 的支援由 kernel support 之後， Guest system 藉由 KVM 模組可以
高效率的執行。然後應用部分 qemu，libvirt 也陸續開發成熟，這樣一條龍已經成熟。
Linux 下的 Virtualization 工具如[雨後春筍般出現](https://www.linux-kvm.org/page/Management_Tools)，

內定的網路虛擬方式，是 Host 採用一個虛擬的 LAN 網段，橋接到實體網絡。

本篇文章就是要記錄，如何使用 NetworkManager 命令模式，設定'虛擬機網路橋接（bridge）'到 host 所在的網段。
參考網頁：[How to create/add network bridge with nmcli](https://www.cyberciti.biz/faq/how-to-add-network-bridge-with-nmcli-networkmanager-on-linux/)

首先要確定套件都安裝了：

	sudo apt-get install qemu-kvm libvirt-daemon-system \
	libvirt-clients virtinst bridge-utils network-manager

可以用 virsh 這個命令看 virtual machine status

	virsh list --all
	 Id   Name   State
	--------------------


注意，上述內容要在同一行，以上設定會使得開機後正確的 disable netfilter，
我們查看一下內定的虛擬網絡方式：

	$> ip link
	1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
		link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00 inet 127.0.0.1/8 scope host lo
		valid_lft forever preferred_lft forever inet6 ::1/128 scope host
		valid_lft forever preferred_lft forever
	2: eno1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
		link/ether 52:54:00:fb:da:a8 brd ff:ff:ff:ff:ff:ff
		inet 192.168.2.21/24 brd 192.168.88.255 scope global dynamic noprefixroute enp1s0
		valid_lft 5614sec preferred_lft 5614sec
		inet6 fe80::5054:ff:fefb:daa8/64 scope link noprefixroute
		valid_lft forever preferred_lft forever
	6: virbr0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP mode DEFAULT group default qlen 1000
	    link/ether 52:54:00:1d:5b:25 brd ff:ff:ff:ff:ff:ff
	7: virbr0-nic: <BROADCAST,MULTICAST> mtu 1500 qdisc fq_codel master virbr0 state DOWN mode DEFAULT group default qlen 1000
	    link/ether 52:54:00:1d:5b:25 brd ff:ff:ff:ff:ff:ff

`enp1s0`是 實體網路 interface.
`virbr0` 跟 `virbr0-nic` 是 KVM 安裝後內建出來的，我們將他們移除：

	$> ip link delete virbr0 type brigde
	$> ip link delete virbr0-nic

現在再看一下我們的網路，後兩項已經移除了：

	$> ip link
	1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
		link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00 inet 127.0.0.1/8 scope host lo
		valid_lft forever preferred_lft forever inet6 ::1/128 scope host
		valid_lft forever preferred_lft forever
	2: eno1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
		link/ether 52:54:00:fb:da:a8 brd ff:ff:ff:ff:ff:ff
		inet 192.168.2.21/24 brd 192.168.88.255 scope global dynamic noprefixroute enp1s0
		valid_lft 5614sec preferred_lft 5614sec
		inet6 fe80::5054:ff:fefb:daa8/64 scope link noprefixroute
		valid_lft forever preferred_lft forever



接下來我們用 nmcli 設定 bridge:

	$tom@atlantic:~/work/rdBlog/myBlog$ nmcli con show
	NAME                UUID                                  TYPE       DEVICE
	Wired connection 1  4e29a074-cb06-40fc-840e-2a10a39cb587  ethernet   eno1
	tomiphone 網路      cef2ed6e-a96c-4163-83bc-fc2ba6c6ba35  bluetooth  --

Add a new bridge:

	tom@atlantic:~/work/rdBlog/myBlog$ nmcli con add type bridge ifname br0
	tom@atlantic:~/work/rdBlog/myBlog$ nmcli con show
	NAME                UUID                                  TYPE       DEVICE
	Wired connection 1  4e29a074-cb06-40fc-840e-2a10a39cb587  ethernet   eno1
	bridge-br0          b2ed19f4-3267-4e11-92ed-c70ae82c9eb5  bridge     br0
	tomiphone 網路      cef2ed6e-a96c-4163-83bc-fc2ba6c6ba35  bluetooth  --


Create a slave interface:

	tom@atlantic:~/work/rdBlog/myBlog$ nmcli con add type bridge-slave ifname eno1 master br0
	連線「bridge-slave-eno1」 (3bc43bb5-6443-4630-a71f-bd405305185a) 已成功新增。
	tom@atlantic:~/work/rdBlog/myBlog$ nmcli con show
	NAME                UUID                                  TYPE       DEVICE
	Wired connection 1  4e29a074-cb06-40fc-840e-2a10a39cb587  ethernet   eno1
	bridge-br0          b2ed19f4-3267-4e11-92ed-c70ae82c9eb5  bridge     br0
	bridge-slave-eno1   3bc43bb5-6443-4630-a71f-bd405305185a  ethernet   --
	tomiphone 網路      cef2ed6e-a96c-4163-83bc-fc2ba6c6ba35  bluetooth  --

看一下目前 ip 仍然是 eno1 上面：

	tom@atlantic:~/work/rdBlog/myBlog$ ip a
	1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
	    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
	    inet 127.0.0.1/8 scope host lo
	       valid_lft forever preferred_lft forever
	    inet6 ::1/128 scope host
	       valid_lft forever preferred_lft forever
	2: eno1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
	    link/ether b4:2e:99:c3:d7:94 brd ff:ff:ff:ff:ff:ff
	    inet 192.168.2.21/24 brd 192.168.2.255 scope global dynamic noprefixroute eno1
	       valid_lft 86005sec preferred_lft 86005sec
	    inet6 2001:b011:7009:3043:4dfb:72df:2866:9182/64 scope global temporary dynamic
	       valid_lft 600sec preferred_lft 600sec
	    inet6 2001:b011:7009:3043:b62e:99ff:fec3:d794/64 scope global dynamic mngtmpaddr noprefixroute
	       valid_lft 600sec preferred_lft 600sec
	    inet6 fe80::b62e:99ff:fec3:d794/64 scope link noprefixroute
	       valid_lft forever preferred_lft forever
	6: br0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default qlen 1000
	    link/ether 32:37:36:40:4d:60 brd ff:ff:ff:ff:ff:ff

打開 bridge-br0:

	tom@atlantic:~/work/rdBlog/myBlog$ nmcli con down 'Wired connection 1'
	tom@atlantic:~/work/rdBlog/myBlog$ nmcli con up bridge-br0


再次用 'ip a' 命令，可以看到`enp1s0`已經 master bridge 到 `br0`, 所以沒有 ip address 了，接下來所以的虛擬機網路都設定到
`br0` 即可, 透過 `virtual-machine-manager` GUI 設定:

![](/images/vmm_using_br0.png)

上圖顯示已經裝好的系統，可以改網路到 `br0`
#
