Title: "LVM-Configuration"
date: 2020-04-29 17:38:00 +0800
Category: Linux
Tags: Logic Volume Manager
Has_Math: True

本篇參考 [鳥哥的 LVM](http://linux.vbird.org/linux_basic/0420quota.php#lvm_whatis)
詳細的內容在鳥哥那邊，介紹得非常清楚。這裡記錄一些特別的性質。

這個儲存架構，需要在安裝時就注意：

	它不能直接擴充目前掛載的 LV 大小，
	必須卸載後進行。所以安裝時讓 root file system
	預留足夠的空間（15G 大概夠用），可以省許多麻煩步驟。

擴充 Logical Volume 的做法是：

	由於內定擴充選項是 Linear 的，
	就是 Append 新的 blocks 到原先的 blocks 後面，
	所以不會擾亂原本的 index，可以無縫接軌的擴充空間。

詳細的操作指令還是參考鳥哥那篇就好，這裡只記錄一些心得。
