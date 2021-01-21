Title: "Debian 10 Pinyin Setup"
date: 2020-06-30 15:15:00 +0800
Category: Setup
Tags: mathjax, Chinese-Input
Has_Math: True



網路上的中文環境設定，從古到今都有。但是如何設定成我習慣的 Mac 拼音輸入法，幾乎找不到，
Debian 10 已經進化得很成熟，只要依照下列方式就可做到：

首先要確定套件都安裝了：

	sudo apt-get install ibus-pinyin ibus-wayland

Restart system.

![](/images/control_panel_add_pinyin.png)

進入控制台，選擇`地區和語言`，+ 新增漢語（Pinyin）項目:

![](/images/control_panel_setup_pinyin_for_traditional_chinese.png)

![](/images/control_panel_setup_pinyin_mode.png)

按下齒輪圖示，進入詳細設定，選擇繁體，就 OK 了


#
