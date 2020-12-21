title: "PHP Extension on Debian 9"
date: 2019-01-15 16:59:00 +0800
Category: Development
Tags: Php
Has_Math: True

本篇敘述 從 Debian 9 打包的 PHP source 來製作一個新的 PHP Extension
假設工作的目錄是 ~/work 在此目錄下指令：

	apt-get source php7.0-dev   ;不用 sudo

會取得 php7.0 source, 這裡拿到 php7.0-7.0.33, 後面都用 `{php-src}` 取代

	cd {php-src}/ext

這裡是所有 extension 的集散地, 而且

<!-- more -->

可以看到工具 ext_skel, 還有骨架參考目錄: skeleton,
用它們產生 extension 骨架, 假設我們的 extension 模組名為 aaa :

	./ext_skel --extname=aaa --skel={php-src}/ext/skeleton

得到一排提示訊息這樣寫：

	Creating directory aaa
	Creating basic files: config.m4 config.w32 .gitignore aaa.c php_aaa.h CREDITS EXPERIMENTAL tests/001.phpt aaa.php [done].
	To use your new extension, you will have to execute the following steps:

	1.  $ cd ..
	2.  $ vi ext/aaa/config.m4
	3.  $ ./buildconf
	4.  $ ./configure --[with|enable]-aaa
	5.  $ make
	6.  $ ./sapi/cli/php -f ext/aaa/aaa.php
	7.  $ vi ext/aaa/aaa.c
	8.  $ make

在這裡就產生 aaa 目錄, 裡面應該有:

	aaa.c  aaa.php  config.m4  config.w32  CREDITS  EXPERIMENTAL  php_aaa.h  tests

附帶紀錄一下我踩到的雷: --skel 下錯位置不會有 .c .h  檔案.
還有提示訊息 2. 在幹嘛? 又有雷, 當然要進去改, 不然之後下 make 後沒有模組出來.
參考[從這篇](http://www.itlnmp.com/431.html), 就是

_...如果你所編寫的擴展如果依賴其它的擴展或者 lib 庫，需要去掉 PHP_ARG_WITH 相關代碼的註釋。
否則，去掉 PHP_ARG_ENABLE 相關代碼段的註釋。
我們編寫的擴展不需要依賴其他的擴展和 lib 庫。因此，我們去掉 PHP_ARG_ENABLE 前面的註釋..._

從這樣：

	dnl If your extension references something external, use with:

	dnl PHP_ARG_WITH(aaa, for aaa support,
	dnl Make sure that the comment is aligned:
	dnl [  --with-aaa             Include aaa support])

	dnl Otherwise use enable:

	dnl PHP_ARG_ENABLE(aaa, whether to enable aaa support,
	dnl Make sure that the comment is aligned:
	dnl [  --enable-aaa           Enable aaa support])

改成這樣：

	dnl If your extension references something external, use with:

	dnl PHP_ARG_WITH(aaa, for aaa support,
	dnl Make sure that the comment is aligned:
	dnl [  --with-aaa             Include aaa support])

	dnl Otherwise use enable:

	PHP_ARG_ENABLE(aaa, whether to enable aaa support,
	Make sure that the comment is aligned:
	[  --enable-aaa           Enable aaa support])

到 aaa 目錄下:

	phpize
	./configure --with-php-config=/usr/bin/php-config7.0
	make
	sudo make install

那個 php config path 怎麼知道的呢？
用 apt-file search php-config 查到的.

下這些指令後, 會 build aaa.so 並且 安裝到 

	Installing shared extensions:     /usr/lib/php/20151012/

在 /etc/php/7.0/cli/conf.d 目錄下都是模組的 .ini 檔案, 在那個目錄下, 
我們手動新增一個 40-aaa.ini :

	[aaa]
	extension=aaa.so

前面的號碼是載入順序, 我們沒有要搶先, 所以選一個後面的號碼(40)即可, 如果您的模組是 xyz,
那麼就新增一個 40-xyz.ini, 內容當然都改為 xyz.

到此為止, 都準備好了, 
前面的 skeleton 產生時, 也順便產生一個測試用的 aaa.php,

run `php aaa.php`:

	Functions available in the test extension:
	confirm_aaa_compiled
	
	Congratulations! You have successfully modified ext/aaa/config.m4. Module aaa is now compiled into PHP.
