Title: Clean Pelican Blogger
Date: 2016-11-16 10:20
Modified: 2016-11-16 19:30
Category: Python
Tags: Pelican, Virtualenv
Has_Math: True


自從 blogger 系統換成 Pelican 之後,因為開始用 Python 處理一些工作，

陸續在 Python 方面增加了許多 packages, 才發現系統的套件版本需要妥善管理.
這時 virtualenv 適當的成為救援投手. 本篇來簡介它的安裝與使用.
<!--more-->

為了印證 virtualenv 的威力, 我們先把系統的 python packages 清乾淨:

	pip freeze | xargs pip uninstall -y
	
注意: 這個動作會把您以前所有安裝的 python packages 從系統清乾淨, 請慎用.

安裝 virtualenv：

	pip install virtualenv

理想上, 此時下 pip list 只會看到：

	tomde-MacBook-Pro:~ tom$ pip list
	Package    Version
	---------- -------
	pip        9.0.1
	setuptools 20.10.1
	virtualenv 15.0.3


作出一個環境, 目錄為 rdBlog (接下來都以此目錄為範例)
把需要安裝的 packages 在此環境內安裝：

	virtualenv rdBlog
	
進入環境很簡單:

	cd rdBlog
	source bin/activate
	
可以發現 prompt 已經不同了, 像這樣:

	(rdBlog) tomde-MacBook-Pro:rdBlog tom$
	
手動寫一個文件 `requirements.txt`, 內容為 pelican 所需的環境:

	appnope==0.1.0
	beautifulsoup4==4.5.1
	blinker==1.4
	decorator==4.0.10
	docutils==0.12
	entrypoints==0.2.2
	feedgenerator==1.9
	ipykernel==4.5.0
	ipython==5.1.0
	ipython-genutils==0.1.0
	ipywidgets==5.2.2
	Jinja2==2.8
	jsonschema==2.5.1
	jupyter==1.0.0
	jupyter-client==4.4.0
	jupyter-console==5.0.0
	jupyter-core==4.2.0
	Markdown==2.6.7
	MarkupSafe==0.23
	mistune==0.7.3
	nbconvert==4.2.0
	nbformat==4.1.0
	notebook==4.2.3
	pelican==3.6.3
	pexpect==4.2.1
	pickleshare==0.7.4
	prompt-toolkit==1.0.9
	ptyprocess==0.5.1
	Pygments==2.1.3
	python-dateutil==2.6.0
	pytz==2016.7
	pyzmq==16.0.1
	qtconsole==4.2.1
	simplegeneric==0.8.1
	six==1.10.0
	terminado==0.6
	tornado==4.4.2
	traitlets==4.3.1
	Unidecode==0.4.19
	wcwidth==0.1.7
	widgetsnbextension==1.2.6

然後在此環境下, 使用：

	pip install -r requirements.txt 
	
即可安裝所有需要的 packages.

此時下：

	pip list

可以看到剛剛列出的 packages 都裝好了.

安裝 `pelican-themes`, 
`pelican-plugins` and `pelican-ipythonnb`：

	cd ..
	git clone https://github.com/11craft/pelican-ipythonnb.git
	git clone https://github.com/getpelican/pelican-plugins
	git clone https://github.com/getpelican/pelican-themes
	cp ~/Documents/pyBlog .
	
然後安裝 gum theme, 看個人喜好選用, 個人是喜歡他的藍色風格

	pelican-themes -i pelican-themes/gum
	(不太確定是否需要 absolute path)
	
到此確認一下我們的目錄：

	:::c
	rdBlog:
		pyBlog
		pelican-plugins
		pelican-themes
		pelican-ipythonnb
		bin
		include
		lib
		share
		~
		
進入我們的 blogger 

	cd pyBlog

一切順利的話,應該可以用

	make

來看看所有的動作.

	make html
	
可以建立 html

	make ssh_upload

可以上傳到您的網站 ！
開心～



ps. 

pip list 時若出現 DEPRECATION: The default format will switch to columns in the future. You can use --format=legacy (or define a list_format in your pip.conf) to disable this warning 的解決方法為:

在 Mac 下 pip.conf 位於 /Users/[your name]/Library/Application Support/pip,
沒有就自己弄一個, 內容為

	[list]
	format = columns
	
