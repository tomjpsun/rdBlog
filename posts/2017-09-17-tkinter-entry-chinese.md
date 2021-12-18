title: "tkinter 輸入中文的問題"
date: 2017-09-17 08:56:00 +0800
Category: Python
Tags: Tkinter, Chinese
Has_Math: True

本篇說明關於在 tkinter (Python 3.x) 下, 使用 tk.Entry 不能輸入中文的現象, 以及解決的方法.
我試過 Python 3.6, 本篇應該對 3.x 都有效

原本的環境:
    Mac OS/X 10.12
    Python 3.6 (/Library/Frameworks/Python.framework/Versions/3.6) 由官方 [https://www.python.org/downloads/](https://www.python.org/downloads/) 下載安裝.
    
如果 .bash_profile 有設定 Python 路徑的話, 像這樣:

    # Setting PATH for Python 3.6
    # The original version is saved in .bash_profile.pysave
    PATH="/Library/Frameworks/Python.framework/Versions/3.6/bin:${PATH}"
    export PATH

我們可以啟動 Python 內建的 idle3, 進入畫面後 可能看到這樣的訊息: 
    
    The version of Tcl/Tk (8.5.9) in use may be unstable # Setting PATH for Python 3.6

這是因為使用了 Apple 內建的 Tcl/Tk, 網路許多文章提到, 此版本還有其他的 bug.
idle3 使用 tkinter (and Tcl/Tk) 所以可以用來鑑定中文輸入是否 OK：此時切換中文輸入, 仍無法輸入中文.

原來在 Python 3.6 安裝時, 會動態連接到 Tcl/Tk, 所以即使重裝 Python 也無法解決, 必須安裝 Active State 提供的 Tcl/Tk

[請不要急著下載這裡的版本](https://www.activestate.com/activetcl/downloads) 

因為這裡提供的 ActiveTcl 8.6.6 仍無法解決此問題 (雖然是官方表格建議的版本)
我安裝 頁面裡的 [ActiveTcl 8.5.18](https://www.activestate.com/activetcl/downloads/thank-you?dl=http://downloads.activestate.com/ActiveTcl/releases/8.5.18.0/ActiveTcl8.5.18.0.298892-macosx10.5-i386-x86_64-threaded.dmg) 才得以解決.

[這篇 Stackoverflow 的文章](https://stackoverflow.com/questions/21129498/idle-warns-against-an-old-tcl-version-even-though-ive-installed-a-newer-version/21181894#21181894)提到_要依序先安裝ActiveTcl,再安裝Python_


以下提供乾淨移除的步驟:( __注意, 這會破壞您原本的 Python 環境, 請謹慎使用__ )

.如何移除從 Python.org 下載的 Python？
    
    $ cd /Library/Frameworks
    $ sudo rm -rf Python.framework

.如何移除 ActiveState 下載的 ActiveTcl?

    $ sudo /Library/Frameworks/Python.framework/Versions/3.5/Resources/Scripts/uninstal

.依序安裝後, 設定 ~/.bash_profile:

    # ActiveState Tk/Tcl
    PATH="/Library/Frameworks/Tcl.framework/Versions/8.5/bin:${PATH}"
    PATH="/Library/Frameworks/Tk.framework/Versions/8.5/bin:${PATH}"
    export PATH
    
    # 下面是 Python.org 安裝時會自動加入的敘述
    # Setting PATH for Python 3.6
    # The original version is saved in .bash_profile.pysave
    PATH="/Library/Frameworks/Python.framework/Versions/3.6/bin:${PATH}"
    export PATH

#
