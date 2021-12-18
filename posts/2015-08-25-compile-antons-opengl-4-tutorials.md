layout: post
title: "Compile Anton's OpenGL 4 Tutorials"
date: 2015-08-25 16:45:55 +0800
comments: true
Category: Opengl
Tags: Learn
Has_Math: True

學習 OpenGL 的兩本經典，除了
[藍皮書](http://www.amazon.com/OpenGL-Superbible-Comprehensive-Tutorial-Reference/dp/0672337479/ref=sr_1_1?s=books&ie=UTF8&qid=1440492484&sr=1-1&keywords=OpenGL)
與
[紅皮書](http://www.amazon.com/OpenGL-Programming-Guide-Official-Learning/dp/0321773039/ref=pd_bxgy_14_img_y)
之外，還有一本很多人也推薦的 [Anton's 教科書](http://www.amazon.com/gp/product/B00LAMQYF2/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=B00LAMQYF2&linkCode=as2&tag=drantger-20&linkId=IJWK2R2DF3S6ACUK)

他的 sample code 放在 [GitHub repo](https://github.com/capnramses/antons_opengl_tutorials_book)

但是，他使用自己攜帶的 lib, 我們在他的 Makefile.osx 可以看到這樣：

	:::bash
	LOC_LIB = $(LIB_PATH)libGLEW.a $(LIB_PATH)libglfw3.a

如果我們自己有 [build GLFW 的 dynamic lib](http://rd.coding-addict.com/blog/2015/01/06/how-to-build-glfw/), 那應該修改成：

	:::bash
	LOC_LIB = $(LIB_PATH)libGLEW.a $(LIB_PATH)libglfw.dylib

就可以透過指令:

	:::bash
	make -f Makefile.osx

來 build samples #
