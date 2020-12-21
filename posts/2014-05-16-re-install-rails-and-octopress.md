layout: post
title: "Re-install Rails and Octopress"
date: 2014-05-16 06:02:34 +0800
comments: true
Category: Rail
Tags: Ror Install, Rbenv
Has_Math: True

Ruby 的套件管理有兩個，rbenv 與 rvm，其中 rbenv 是在 User 目錄下安裝，比較不會**污染**系統。（常常用暴力安裝在系統的東西，可能會因為 Mac OS 修復權限時弄亂環境，造成麻煩）。

因為系統轉移，趁機會裝新的 Ruby(2.1.2)，順便改用 rbenv，以下是衝撞了許多錯誤後整理出來的安裝順序，當做日後參考：
<!--More-->

* install brew (will use /usr/local and add user to its group)

		ruby -e "$(curl -fsSL https://raw.github.com/Homebrew/homebrew/go/install)"
		# check brew
		brew doctor

* uninstall rvm. Maybe rbenv cannot co-exist with rvm.

		sudo rvm implode

* remove anything about rvm in .bash_profile.

	手動移除相關的 script descriptions.

* reinstall Xcode command tools:

	Xcode menu -> Xcode -> Open developer tool ->
	More Developer Tool -> Download the recent Xcode Command Line Tools.

* install rbenv, Check out rbenv into ~/.rbenv.

		brew install rbenv ruby-build

* Add ~/.rbenv/bin to your $PATH for access to the rbenv command-line utility.

		echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bash_profile

* Add rbenv init to your shell to enable shims and autocompletion.

		echo 'eval "$(rbenv init -)"' >> ~/.bash_profile
		source ~/.bash_profile

* work-around some ruby install problem about openssl

		rbenv install 2.1.2
		# tell rbenv to update the shim
		rbenv hash

* using 2.1.2 in local, thus rake install will be local

		rbenv local 2.1.2
		gem install rails

* reinstall octopress

		cd ~/Document/blenderBlog
		gem install bundler
		rbenv rehash
		bundle install
		rake install

終於 Octopress 又回來了！
#
