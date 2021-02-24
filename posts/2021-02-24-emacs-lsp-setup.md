title: Emacs LSP Setup
slug: emacs_with_language_server
date: 2021-02-24 12:33:45 UTC+08:00
type: text

採用 Emacs + Language Server 作為稱手的 IDE, 需 version 27 以上的 Emacs, 以滿足相關 MELPA 套件版本的依賴關係。
Debian 10（Buster）目前僅僅提供 version 26, 所以只好自己 build:

##Emacs-28##

從 Emacs GitHub 下載
[https://github.com/emacs-mirror/emacs](https://github.com/emacs-mirror/emacs)

安裝需要的套件:

	sudo apt install -y texinfo libxpm-dev libgif-dev

因為根據記憶來寫，有些遺漏就請自行補上，./configure 都會提示，尋找缺件的 -dev 套件就對了，
ver 28 已經把 mail 套件列為 optional，因此只有 warning，不安裝也可以繼續 make：

	cd emacs
	./autogen.sh
	./confgure
	make

##Emacs init.el#

在 init.el 加入：

	;; 自動安裝缺件
	(package-initialize)
	(unless package-archive-contents (package-refresh-contents))
	(package-install-selected-packages)

	;; 指定需要的套件
	(custom-set-variables
	 '(next-error-recenter 0)
	 '(package-selected-packages
	   '(lsp-ivy lsp-mode lsp-treemacs lsp-ui treemacs auto-complete auto-complete-clang-async)))

	(require 'lsp-mode)
	(add-hook 'c++-mode-hook #'lsp)


##Clang-11##

LLVM/CLANG 也需要非官方提供的新版本，我們從 [https://apt.llvm.org/](https://apt.llvm.org/) 下載 clang-11，
我們將下列 <version number> 代入 11，就可以順利安裝 clang-11

	wget https://apt.llvm.org/llvm.sh
	chmod +x llvm.sh
	sudo ./llvm.sh <version number>

以上動作會在 /etc/apt/source.list 加上：

	deb http://apt.llvm.org/buster/ llvm-toolchain-buster-11 main
    # deb-src http://apt.llvm.org/buster/ llvm-toolchain-buster-11 main

主要是幫我們安裝 clangd, 之後 Emacs lsp-mode 會自動尋找 clangd。

##使用##

在需要 index 的 project 目錄下提供 compile_commands.json，用 bear make 可造出來。
開啟 project 下的 .cpp source，熱鍵：

	<M-.> 尋找游標所在處的變數定義
	<M-,> 回到前一次停留的地方

參考文件：
C++代碼索引工具現狀:[https://maskray.me/blog/2017-12-03-c++-language-server-cquery](https://maskray.me/blog/2017-12-03-c++-language-server-cquery)


#
