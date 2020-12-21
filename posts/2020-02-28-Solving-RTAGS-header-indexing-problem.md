Title: "Solving RTAGS Headers Indexing Problem"
date: 2020-02-28 16:47:00 +0800
Category: Emacs
Tags: Rtags
Has_Math: True

最近發現 RTags 遇到 project 內的 headers 無法查閱的現象, 尤其在以 C++ template 大量使用的
專案中, 這個現象使得 _Source Code Browsing_ 發生困擾.

原因是因為如果用 cmake -DCMAKE_EXPORT_COMPILE_COMMANDS=1 .. 是不會將用到的 include headers
放進 compilation database (JSON file) 裡面，

但是這個 [compdb](https://github.com/Sarcasm/compdb) 專案解決了這個問題, 只要將原本產生的
compile_commands.json 餵進去就可以將 compilation database 加上 include headers 了.

以 [Customize Build MLPack](http://rd.coding-addict.com/customize-build-mlpack.html) 為例,
我們在 project root dir 下已經建立 build 子目錄, 也跑過 cmake -DCMKAE_EXPORT_COMPILE_COMMANDS=1
了, 那麼在 project root dir 下指令：

	compdb -p build/ list > compile_commands.json

就可以將 build/compile_commands.json 加上 headers 資訊, 並放在 project roog dir 下,
接著在 project root dir 重新 parse 即可:

	rc -J

這個使用問題發生一段時間了, 現在終於解決, Emacs + RTags 真是讓人開心！
