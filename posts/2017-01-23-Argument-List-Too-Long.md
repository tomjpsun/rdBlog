title: "Argument List Too Long"
date: 2017-01-19- 17:21:22 +0800
Category: Misc
Tags: Macos X, Command Line
Has_Math: True

一般在 Mac 下, 壓縮檔案只要選取後, 滑鼠右鍵就可以出現選單, 從裡面選擇壓縮 xxx 個項目即可.
但是, 如果 zip 需要加密, 就得用 command line.

但是, 我處理 15,000 個檔案時, 無法完成的錯誤

	# 想要將目前目錄下所有檔案 壓縮起來 並加密....
	> zip -e xxx.zip *.wav
	-bash: /usr/bin/zip: Argument list too long

find 也失敗：

	> find ./ -name *.wav -print0 | xargs zip -e wav.zip -
	-bash: /usr/bin/find: Argument list too long
	
單單用 ls 卻可以, 故特別記錄下來：

	> ls | xargs zip -e wav.zip -
	Enter password:
	
成功！！
