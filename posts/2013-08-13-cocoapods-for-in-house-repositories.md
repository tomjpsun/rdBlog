title: "Cocoapods for In-House repositories"
date: 2013-08-13 16:58
comments: true
Category: Ios
Tags: Cocoapods
Has_Math: True

本文紀錄關於 pod spec 的一些概念：

__概念：PodSpec 可以跟 Repository Source 分開放不同目錄__

例如：Github 上面的 PodSpec 就是單純的 PodSpec 的集合。 所以如果情況需要的話，可以做出私有的 repo，放在私有的 git repo server 上面，只要 podspec 裡面有 source 的 git 路徑即可。

<!-- more -->

A PodSpec example:

	:::ruby
	Pod::Spec.new do |s|
		s.name         = "SimpleGL"
		s.version      = "0.0.2"
	 	s.summary      = "Alternative OPGL startup template simpler than the Xcode built-in."
	 	s.homepage     = "http://github.com/tomjpsun/SimpleGL"
	 	s.license      = { :type => 'MIT', :file => 'LICENSE.txt' }
	 	s.author       = { "Tom Sun" => "tomjpsun@gmail.com" }
	 	s.source       = { :git => "git@private-repo-server:SimpleGL.git", :tag => "0.0.2" }
	 	s.platform     = :ios, '5.0'
	 	s.source_files = 'TestOGL/GLView.{h,m}'
	 	s.frameworks = 'OpenGLES', 'QuartzCore', 'CoreGraphics'
	 	s.requires_arc = true
	 end



這裡的 s.source 用 ruby hash 表示，:git 的值就是 git@private-repo-server:SimpleGL.git，其中 :git 是 [pod specification 的 source](http://docs.cocoapods.org/specification.html#source) 選項中，內定的識別字。 我們在這裡，就指到自有的 git repository, :tag 是標明版本編號，後面會說明這個部分。

至於這個 podspec file，就可以另外放在其他地方，如自行建立的 Private-Cocoapods 目錄等等。
概念：可同時允許多個 Repositories 並存。

安裝 Cocoapods 時，會在使用者目錄下 ~/.cocoapods 把 [Github 上面的 PodSpec](https://github.com/CocoaPods/Specs) 都裝到 master 目錄下。

使用’pod repo add’ 指令，可以增加 repo。 也就是，可以增加私有的 repo，與 Github 上面的 PodSpec 並存。

__概念：使用 tag 標示版本__

Cocoapod 依據 podspec 的指示，到 git repo 抓 source 時，會依 tag 來尋找正確的版次，所以在我們提供的 source git 裡面，要用 tag 標示版次，使用 [semantic versioning format](http://semver.org/)

至於整個過程，網路上有許多教學資料，這裡列出我找到的參考資料。

參考資料：

Private Cocoapods <https://coderwall.com/p/7ucsva>

建立 podspec file 影片教學 <http://nsscreencast.com/episodes/28-creating-a-cocoapod>

Creating a Pod Spec <http://theonlylars.com/blog/2013/01/20/cocoapods-creating-a-pod-spec/>

fcamel blog: Semantic Versioning Format <http://fcamel-life.blogspot.tw/2011/07/semantic-version.html>
