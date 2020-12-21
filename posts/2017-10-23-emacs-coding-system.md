title: "Emacs Coding System 101"
date: 2017-10-23 20:56:00 +0800
Category: Emacs
Tags: Coding-System
Has_Math: True

Emacs 有很清楚的 coding system 指令, 目前解決下載 Big5 文件, 轉換成 utf-8 的作法, 大概是用這幾個指令就可以搞定：

`C-x` `h`: buffer 全選

`M-x` `revert-buffer-with-coding-system` `RET`: 強制轉換成我們要的 coding system

`C-x` `RET` `c` _coding_ `RET`: 準備接下來的指令使用 _coding_ 完成, 然後 `C-x` `C-s` 存檔.

`C-h` `v` 然後觀察 `buffer-file-coding-system` 變數, 即可知道現在 emacs 用什麼 codign system 來解釋 buffer data.

參考網站：

[http://ergoemacs.org/emacs/emacs_encoding_decoding_faq.html](http://ergoemacs.org/emacs/emacs_encoding_decoding_faq.html)

[https://www.gnu.org/software/emacs/manual/html_node/emacs/Text-Coding.html](https://www.gnu.org/software/emacs/manual/html_node/emacs/Text-Coding.html)





