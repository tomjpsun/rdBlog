layout: post
title: "How to build GLFW"
date: 2015-01-06 13:37:37 +0800
comments: true
Category: Opengl
Tags: Build,Glfw
Has_Math: True

[GLFW](http://www.glfw.org/) 是一套 OpenGL 的 Library --- 因為直接 OpenGL 使用會很瑣碎。所以通常大家都會透過高階一點的 Library 來使用 OpenGL。其中，歷史悠久的 glut 已經被廣泛的使用，但是 GLFW 定位在支援 OpenGL 3.2 以上, 還有 OpenGL ES 等等的好處.我們這一篇介紹時, GLFW 的釋出版本為 3.0.4,以下就介紹如何在 OS X 10.10 下，安裝一個起始專案ㄔ

__更新:__ 2015/08 月份已經到 3.1 版了
<!--More-->
__前置環境:__

* Xcode 6.1.1
* CMake 3.1.0

Xcode 可以到 Apple developer 網站下載.
要先確定環境正確，請下命令：

>sudo xcode-select -s /Applications/Xcode.app/Contents/Developer/

若省略這步驟，可能會發生 cmake 警告：No CMAKE_C_COMPILER could be found

CMake 可以先安裝圖形介面的 App, 再於主選單下選擇
>`Tools` | `Install For Command Line Use`

安裝命令列模式的 link, 筆者這邊是指定裝到 `/usr/local/bin`下面, 安裝好了 可以用
> which cmake

來驗證，應該要出現：
>/usr/local/bin/cmake

__下載 GLFW:__ [http://www.glfw.org/](http://www.glfw.org/)

會在本目錄下安裝 glfw, 因為想保留 git 下載的 source, 我們另外在同一層建立 build-glfw 目錄(與 glfw 為 sibling)

進入 build-glfw 下命令：
>cmake -DBUILD_SHARED_LIBS=on -GXcode ../glfw

其中 BUILD_SHARED_LIBS 是產生動態程式庫 .dylib 的意思，Default 為 off.
-GXcode 告訴 Cmake 產生 Xcode 專案.

成功後的畫面是：

	Mac-mini:build-glfw tom$ ls
	CMakeCache.txt		GLFW.xcodeproj		examples
	CMakeFiles		cmake_install.cmake	install_manifest.txt
	CMakeScripts		cmake_uninstall.cmake	src
	GLFW.build		docs			tests

可以看到產生出 GLFW.xcodeproj, 用 Xcode 打開這個 project, schema 選擇 install, 就可以叫 Xcode 開始 build GLFW 了(Cmd-B).
產生的 lib 位於 `/usr/local/lib` 下面：

	Mac-mini:build-glfw tom$ ls -al /usr/local/lib/libglfw*
	-rwxr-xr-x  1 tom  admin  140288  8 25 16:57 /usr/local/lib/libglfw.3.1.dylib
	lrwxr-xr-x  1 tom  admin      17  1 12  2015 /usr/local/lib/libglfw.3.dylib -> 	libglfw.3.1.dylib
	lrwxr-xr-x  1 tom  admin      15  1 12  2015 /usr/local/lib/libglfw.dylib -> 	libglfw.3.dylib

產生的 header files 會放到 `/usr/local/include/GLFW`

我們可以觀察一下 各個 scheme 的相依關係,
選擇 `target` | `Build Phase` | `Target Dependencies` 可以看到
__install__ -> __ALL_BUILD__ -> __所有的 targets__(包含 __glfw__)
而 __glfw__ 的 `Build Settings` | `Linking` 需要 `Cocoa` `OpenGL` `IOKit` `CoreVideo` 等 frameworks:

<img src="http://coding-addict.com/pictures/rd/glfw xcode with frameworks.png" alt="Drawing" style="width: 800px;"/>


__到此為止，GLFW lib 已經可以編譯了__, 接下來就是把它與我們的專案合併在 Xcode workspace 即可.

用 Xcode 新生一個 OS X command line project，名為`glfw_template`, 把 main.c 內容換成：

	:::Objective-C

	#import <Foundation/Foundation.h>

	#include "GLFW/glfw3.h"

	static void error_callback(int error, const char* description)
	{
	    fputs(description, stderr);
	}
	static void key_callback(GLFWwindow* window, int key, int scancode, int action, int mods)
	{
	    if (key == GLFW_KEY_ESCAPE && action == GLFW_PRESS)
	        glfwSetWindowShouldClose(window, GL_TRUE);
	}
	int main(void)
	{
	    GLFWwindow* window;
	    glfwSetErrorCallback(error_callback);
	    if (!glfwInit())
	        exit(EXIT_FAILURE);
	    window = glfwCreateWindow(640, 480, "Simple example", NULL, NULL);
	    if (!window)
	    {
	        glfwTerminate();
	        exit(EXIT_FAILURE);
	    }
	    glfwMakeContextCurrent(window);
	    glfwSwapInterval(1);
	    glfwSetKeyCallback(window, key_callback);
	    while (!glfwWindowShouldClose(window))
	    {
	        float ratio;
	        int width, height;
	        glfwGetFramebufferSize(window, &width, &height);
	        ratio = width / (float) height;
	        glViewport(0, 0, width, height);
	        glClear(GL_COLOR_BUFFER_BIT);
	        glMatrixMode(GL_PROJECTION);
	        glLoadIdentity();
	        glOrtho(-ratio, ratio, -1.f, 1.f, 1.f, -1.f);
	        glMatrixMode(GL_MODELVIEW);
	        glLoadIdentity();
	        glRotatef((float) glfwGetTime() * 50.f, 0.f, 0.f, 1.f);
	        glBegin(GL_TRIANGLES);
	        glColor3f(1.f, 0.f, 0.f);
	        glVertex3f(-0.6f, -0.4f, 0.f);
	        glColor3f(0.f, 1.f, 0.f);
	        glVertex3f(0.6f, -0.4f, 0.f);
	        glColor3f(0.f, 0.f, 1.f);
	        glVertex3f(0.f, 0.6f, 0.f);
	        glEnd();
	        glfwSwapBuffers(window);
	        glfwPollEvents();
	    }
	    glfwDestroyWindow(window);
	    glfwTerminate();
	    exit(EXIT_SUCCESS);
	}



加入需要的 lib:

<img src="http://coding-addict.com/pictures/rd/add libglfw.png" alt="Drawing" style="width: 1500px;"/>

最後如圖加上 __Library Search Path__ (recursive) : `/usr/local/lib`

<img src="http://coding-addict.com/pictures/rd/build glfw add include.png" alt="Drawing" style="width: 1500px;"/>


還有 __User Header Search Path__ : `/usr/local/include`

<img src="http://coding-addict.com/pictures/rd/Setup include dir for GLFW.png" alt="Drawing" style="width: 500px;"/>

這樣，就可以建立出我們的 starting project 了，run 此專案, 應該會有一個黑色的 window, 裡面就是 OpenGL context 了.


