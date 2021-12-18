title: "Android re-sign App"
date: 2017-06-28 22:30:00 +0800
Category: Android
Tags: Sign
Has_Math: True

因為大意, 把前一次的 App signing key 弄丟了,
google 到一個 [blogger](http://blog.csdn.net/wei18359100306/article/details/40678253), 節錄一段：
	
	如果你的签名丢了怎么办，有两种修复方式：

     1.包名不变的情况下，改签名    -----> 牺牲用户，成全自己

         优点：你在各个应用市场的排名还是原来的排名（比如你原先在应用市场排名第一，这样子改还是第一，适合应用市场排名高的 

         缺点：无法覆盖安装，即只能提醒用户把原来的程序给卸了，安装新的应用程序（用户体验差）

     2.改包名 ，重新签名    ----->   牺牲自己，成全用户
 
但是改簽名後, 曾經安裝過的機器就要特別用 adb 清理一下:

	$ adb uninstall com.xxx.package
	
否則會顯示 "無法安裝" 的訊息. (com.xxx.package 是我們的 app package name) 

_adb_ uninstall 會清理系統內已經 cache 的套件名稱, 
第一次 OK, 沒有錯誤訊息, 但是重複這命令, 會因為找不到 package name 而告訴你：

	Failure [DELETE_FAILED_INTERNAL_ERROR]

簽名檔要注意: Android 7.0 才引入 V2, 我們的 App 若是 7.0 以前的版本, 最下面的 V1, V2 都要勾選:

<img src="https://i.stack.imgur.com/Dijv8.png" alt="Drawing" style="width: 500px;"/>

否則會看到這個訊息：

	Failure [INSTALL_PARSE_FAILED_NO_CERTIFICATES]

表示不認得您的簽名.


以上列出的是一些使用經驗, 其實沒有什麼技術含量, 提醒自己以後快速處理掉, 別在這裡搞很久.

