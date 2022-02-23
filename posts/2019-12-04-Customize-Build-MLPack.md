Title: "Customize Build MLPack"
date: 2019-12-04 10:30:00 +0800
Category: Machine Learning
Tags: Framework, Library
Has_Math: True

最近發現有趣的專案 [mlpack](https://www.mlpack.org/)

這個 library 把 ML 的演算法設計成一個個的 模組, 可以直接用 shell command 呼叫, 因為底層透過 `armadillo` 運算,
加上專案本身用 C++ 實作, 所以執行效能應該很好, 算是輕快等級的工具.
官網[https://www.mlpack.org/](https://www.mlpack.org/)可以下載 source

### Build on Debian ###

	sudo apt install libomp-dev libmlpack-dev clang libarmadillo*

Note：本專案並沒有通過在 Mac 上的驗證（make test），不建議在 Mac 環境使用。

build 指令, 在 source root 下:

	mkdir build
	cd build
	CC=clang CXX=clang++ cmake -DCMAKE_EXPORT_COMPILE_COMMANDS=1 ..
	sudo make install

不用守在機器前面啦, 大約經過吃一頓飯的時間才可以完成.

# experience MLPack #

下載 [https://github.com/mlpack/examples.git](https://github.com/mlpack/examples.git),
可以體驗一下如何使用，首先到 tools/ 目錄下，開一個虛擬 python 環境
(下面示範，是已經安裝了 python3 之後)：

	cd tools/
	# 建立虛擬環境
	python3 -m venv ENV
	# 進入虛擬環境
	source ENV/bin/activate
	# 安裝需要的套件
	pip install tqdm requests
	# 開始下載
	python3 download_data_set.py

Readme 說明有許多展示範例，舉例來說，我們對 mnist-cnn 有興趣，那就進入該目錄：

	cd mnist_cnn

修改 Makefile 使用 OpemMP lib (以下為 diff 表示)：

	-CXXFLAGS += -std=c++11 -Wall -Wextra -O3 -DNDEBUG -fopenmp
	+CXXFLAGS += -std=c++11 -Wall -Wextra -O3 -DNDEBUG -Xpreprocessor -fopenmp
    # Use these CXXFLAGS instead if you want to compile with debugging symbols and
    # without optimizations.
    # CXXFLAGS += -std=c++11 -Wall -Wextra -g -O0
    -LDFLAGS  += -fopenmp
	+LDFLAGS  += -lomp

注意程式碼裡面，使用到 "../data/train.csv" 要改名為 "../data/mnist_train.csv"
"../data/test.csv" 要改名為 "../data/mnist_test.csv"

改好後就可以順利 build & run 了#



### Nvidia Acceleration ###


如果您有 NVidia 繪圖卡, 並且在 Debian 10 安裝了 CUDA driver 的話，那就可以用 Nvidia 繪圖晶片加速。在 [Armadillo Documents](http://arma.sourceforge.net/faq.html#linking) 的討論有提到，將 `-lblas` 換作 `-lopenblas` 就可以用 OpenBLAS 置換 BLAS (Armadillo' default BLAS library)，同理 `-lnvblas` 應該也可以使 NVBLAS drop in replace BLAS，請記得安裝 nvblas 的 package.

2021-01-26 更新： NVIDIA 提供 nvidia-cuda-toolkit version 11 與 12, 然而 cuDNN 只出到 11,
所以我們安裝 cuda toolkit 11, 還有, NVIDIA 官網提供的 .deb package 將 CUDA library
放在 /usr/local/cuda/lib64 下, 也包含新版的 libnvblas, 等下我們會把此路徑告訴 mlpack.

# nvblas.conf #

那麼，怎麼將 nvblas 加到我們的 `mlpack` build 呢？ 在 [NVBLAS](https://docs.nvidia.com/cuda/nvblas/index.html) 有提到,
要提供 nvblas.conf 檔案, 並且由環境變數 _NVBLAS_CONFIG_FILE_ 指名到該檔案.

	NVBLAS_CONFIG_FILE=/etc/nvblas.conf

/etc/nvblas.conf 內容為 [NVBLAS 提供的 nvblas.conf 參考](https://docs.nvidia.com/cuda/nvblas/index.html#configuration_example)
把它抓下來放在我們的 /etc/ 目錄下.

2022/2/19 修改：
nvblas.conf 裏面
NVBLAS_CPU_BLAS_LIB 改爲  /usr/lib/x86_64-linux-gnu/openblas-openmp/libopenblas.so,
配合安裝 libopenblas-openmp-dev package.

接下來修改 mlpack 的 CMakeList.txt:

	-    set(COMPILER_SUPPORT_LIBRARIES "")
	+    set(COMPILER_SUPPORT_LIBRARIES "nvblas")
	+    set(MLPACK_LIBRARIES  "nvblas")
	+    link_directories(/usr/local/cuda/lib64)


COMPILER_SUPPORT_LIBRARIES 是給 mlpack_xxx CLI linking 用的
MLPACK_LIBRARIES 是給 libmlpack.so linkin 用的。
link_directories 是增加 link 需要尋找的 lib path (LD_LIBRARY_PATH),
然後開始 build:

	:::sh
	mkdir build
	cd build
	cmake -DCMAKE_EXPORT_COMPILE_COMMANDS=1 ..
	sudo NVBLAS_CONFIG_FILE=/etc/nvblas.conf make install

檢查一下是否真的有效：

	:::sh
	$ ldd /usr/local/lib/libmlpack.so | grep nvblas
	  libnvblas.so.11 => /usr/local/cuda-11.1/targets/x86_64-linux/lib/libnvblas.so.11 (0x00007f8b3e9a5000)



如同前面的方式，下載 mlpack-examples，並且用 mnist_cnn 來試驗：

	#Makefile 修改, 將這行：
	LIBS_NAME := armadillo mlpack boost_serialization

	改為這樣：
	LIBS_NAME := nvblas armadillo mlpack boost_serialization

make 成功後，下 ldd 檢驗 NVBLAS 是否已經進來？

     $ ldd mnist_cnn | grep nvblas
	   libnvblas.so.11 => /usr/local/cuda/lib64/libnvblas.so.11 (0x00007fbec4fed000)
