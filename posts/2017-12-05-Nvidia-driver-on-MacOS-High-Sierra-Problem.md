title: "Nvidia driver on MacOS High Sierra Problem"
date: 2017-12-05 08:56:00 +0800
Category: Graphics
Tags: Settings, Macos
Has_Math: True

自從更新到 MacOS High Sierra 之後, 就發現 _控制台_ 裡面的 CUDA 設定一直警告 "driver Update Required",
但是卻無法逕行更新.

[這篇](https://devtalk.nvidia.com/default/topic/1025945/mac-cuda-9-0-driver-fully-compatible-with-macos-high-sierra-10-13-error-quot-update-required-quot-solved-/)有詳細的說明：大致上就是 Apple 目前的原生 driver 無法驅動 Nvidia 顯卡, 必須安裝 Nvidia 提供的 driver才行.

該文章已經有列出解決方案, 但是它的 download link 是給 High Sierra 10.13.1 beta 版本的, 目前已經無法被正式的 High Sierra 10.13.1 使用, 必須找到適合的 driver, 以下節錄並修正其步驟：

1. Uninstall 原本的 driver:
   清理下列目錄 (若沒有該目錄就跳過):
   
	   - /System/Library/Extensions/CUDA.kext
	   - /Library/Frameworks/CUDA.framework
	   - /Library/LaunchAgents/com.nvidia.CUDASoftwareUpdate.plist
	   - /Library/PreferencePanes/CUDA/Preferences.prefPane
	   - /System/Library/StartupItems/CUDA/
	   - /Developer/NVIDIA/CUDA-5.0
	   - /usr/local/cuda
	   
2. 重開機：不能省此步驟

3. 下載 Nvidia web driver 並安裝:
   [https://www.tonymacx86.com/nvidia-drivers/](https://www.tonymacx86.com/nvidia-drivers/)
   必要時, 要允許``安全性與隱私``(狂按允許幾百下) 
   
4. 重開機：不確定是否可以省略此步驟.

5. [下載 CUDA driver](http://www.nvidia.com/object/mac-driver-archive.html) 並安裝。

6. 至此已經成功, 我是開 Blender->File->User Preferences->System Tab 可以看到左下出現了 CUDA 選項.

<img src="http://coding-addict.com/pictures/rd/Nvida_web_driver_install_success.png" alt="Drawing" style="width: 680px;"/>


   




