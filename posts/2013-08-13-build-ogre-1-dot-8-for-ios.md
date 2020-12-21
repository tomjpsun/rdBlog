title: "Build Ogre 1.8 for iOS"
date: 2013-08-13 17:07
Category: Ogre
Tags: Build
Has_Math: True


Ogre 是開源的 3D 引擎，支援 Windows/Linux/Mac OS.

之前手動 build Ogre v1.7 for iOS 遇到很多問題。 Ogre v1.8 對於 iOS 的支援，改善了許多，這裡依序列出過程：

<!-- more -->

##準備：##

* CMake 2.8.8 以上，才支援 Xcode 4.3；要安裝 command line

* 安裝 Mercurial 套件，抓 source 用的

* 從 Ogre download link 抓取 ```Ogre_Xcod4_Templates_20130325.pkg.zip``` 與 ```Ogre_iOS_6.0_Dependencies_20121223.dmg```

* 抓 Mercurial repo 的 Ogre source: ```hg clone http://bitbucket.org/sinbad/ogre/``` 然後切到分支 ```hg update v1-8```

* 將 ```Ogre_iOS_6.0_Dependencies_20121223.dmg``` 掛上，直接抓 iOSDependencies 到 Ogre source tree root。

##開始 build，這裡分兩種：##

**Building for running the sample projects.**

如果你要 trace Ogre source 的話，這種方式方便你在 Xcode 內 trace.

**Building for compatibility with Ogre’s “iPhone OS” Xcode project template.**

採用這種 build 方式，還要先下載 Command Line Tools from Xcode Downloads Preference，這種方式會整理出 Ogre SDK。若要開發 Ogre Application，適合用這種 build 方式。

[build 參考文件](http://www.ogre3d.org/tikiwiki/tiki-index.php?page=Building%20From%20Source%20-%20iPhone&redirectpage=Building%20From%20Source%20%28for%20iPhone%29)

##Building for running the sample projects##

* 在 Ogre source tree root 下指令：

```mkdir build && cd build cmake -D OGRE_BUILD_PLATFORM_APPLE_IOS=1 -G Xcode ..```

* 在 build 目錄下會產生 OGRE.xcodeproj ，用 Xcode 開啟 build scheme 選 SampleBrowser。

**注意：**```ALL_BUILD``` 在 iOS 下會有問題，不要用。

* 等幾分鐘 build 過程就 OK！

##Building for compatibility with Ogre’s “iPhone OS” Xcode project template##

* 在 Ogre source tree root 下用 [build 參考文件](http://www.ogre3d.org/tikiwiki/tiki-index.php?page=Building%20From%20Source%20-%20iPhone&redirectpage=Building%20From%20Source%20%28for%20iPhone%29) 的 script，其中 BUILD 變數可以自己更改。 例如：我的環境改為BUILD=`pwd`/iOSsdk。將來，我在 template 產生後，就將 OGRE_SDK_ROOT 指向該目錄。

![Image](http://coding-addict.com/pictures/blogger/ogre_script_variables.png)

* 在 source tree root 下執行該 script，然後等幾分鐘。

* 安裝Ogre_Xcod4_Templates_20130325.pkg.zip，會在 Xcode template 內新增 Ogre template (iOS/OS X 都有)

* 從 Xcode 內新增一個 project，選 iOS 的 Ogre Application。

* 如圖設定 Xcode 的 build settings，手動新增OGRE_SDK_ROOT 並指定為剛剛的 $BUILD 目錄位置。

![Image](http://coding-addict.com/pictures/blogger/ogre_sdk_root.png)

* build project 得到一個 Ogre App!

##結語##

Ogre 1.8 後對於 iOS build 的支援已經很好用了。接下來就是學習如何使用 Ogre enging 來製作您的 3D App 了！
