title: "Build rtags"
date: 2018-02-09 23:00:00 +0800
Category: Emacs
Tags: C++ Code Browser, Development
Has_Math: True

Gnu Global 用在 parse C 程式碼非常快速好用, 但是遇到 C++ overloading function 時, 有時會失去準頭, 找不到正確的 函數定義.
[Rtags](https://github.com/Andersbakken/rtags) 採用 clang 前端處理(libclang API), 表現非常的好, Rtags 專案也與 emacs 高度整合, 提供 rtags.el 插件, 
本篇介紹如何使用 Rtags. 我們的環境是 Debian 9.0 Stretch.

我們打算使用 clang-3.9, 雖然 rtag 網站的最低需求是 clang >= 3.3, 但是
clang-3.9 有 scan-build 這支工具, 建立 compile environment 會很方便,
因此我們在這就直接使用 clang-3.9 套件.

## Prerequisiting Packages ##

	$ sudo apt-get install clang-3.9 libclang-3.9-dev libcppunit-dev pkg-config

## Download rtags##

	note: download with recursive is required
	$ git clone --recursive https://github.com/Andersbakken/rtags.git
	$ cd rtags/
	$ mkdir build
	$ cd build
	$ cmake ..
	$ make
	$ make install
	
## Start RDM##

	# daemon 模式啟動, Log 檔為 ~/rdm.log(可以自訂)
	$ rdm --daemon -L ~/rdm.log
	
## Prepare for emacs##

	$ cp -a /usr/local/share/emacs/site-lisp/rtags ~/.emacs.d
	
add the following lines in your init.el:

	; search all path under site-lisp
	(let ((default-directory  "~/.emacs.d/site-lisp/"))
    (normal-top-level-add-subdirs-to-load-path))


	(add-to-list 'load-path "~/.emacs.d/rtags/")
	(eval-after-load 'cc-mode
	  '(progn
	     (require 'rtags)
	     (mapc (lambda (x)
	             (define-key c-mode-base-map
	               (kbd (concat "C-z " (car x))) (cdr x)))
	           '(("." . rtags-find-symbol-at-point)
	             ("," . rtags-find-references-at-point)
	             ("v" . rtags-find-virtuals-at-point)
	             ("V" . rtags-print-enum-value-at-point)
	             ("/" . rtags-find-all-references-at-point)
	             ("Y" . rtags-cycle-overlays-on-screen)
	             (">" . rtags-find-symbol)
	             ("<" . rtags-find-references)
	             ("-" . rtags-location-stack-back)
	             ("+" . rtags-location-stack-forward)
	             ("D" . rtags-diagnostics)
	             ("G" . rtags-guess-function-at-point)
	             ("p" . rtags-set-current-project)
	             ("P" . rtags-print-dependencies)
	             ("e" . rtags-reparse-file)
	             ("E" . rtags-preprocess-file)
	             ("R" . rtags-rename-symbol)
	             ("M" . rtags-symbol-info)
	             ("S" . rtags-display-summary)
	             ("O" . rtags-goto-offset)
	             (";" . rtags-find-file)
	             ("F" . rtags-fixit)
	             ("X" . rtags-fix-fixit-at-point)
	             ("B" . rtags-show-rtags-buffer)
	             ("I" . rtags-imenu)
	             ("T" . rtags-taglist)))))
	
## Tell rdm to parse your project:##

makefile:

	make -nk | rc -c -

cmake:

	cmake -DCMAKE_EXPORT_COMPILE_COMMANDS=1 .
	rc -J
	
其餘請到 [rtags 官方文件](https://github.com/Andersbakken/rtags) 查詢
#



	
	


