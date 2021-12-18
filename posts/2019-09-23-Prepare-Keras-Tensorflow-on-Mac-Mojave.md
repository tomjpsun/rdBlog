Title: "Prepare Keras/Tensorflow on Mac/Mojave"
date: 2019-09-23 16:50:00 +0800
Category: Machine Learning
Tags: Tensorflow, Installation
Has_Math: True

本文來自於[Deep Learning With Python by François Chollet](https://www.amazon.com/Deep-Learning-Python-Francois-Chollet/dp/1617294438)
一書所提到的的安裝環境, 因為有些 Python 套件版本已經比本書還要新, 故使用時會有一些煩人的訊息, 經過試驗後, 我們在這裏整理出比較可用的環境.

<!-- more -->

首先是 pip 安裝 keras 並不會自動安裝 tensorflow, 反之亦然. 所以它們兩個要各別用 pip 安裝.

如果

	> pip install tensorflow

得到 tensorflow-1.14 那麼他跟 numpy-1.17 合用會有問題, log 會一直出現 [FutureWarning...](https://github.com/tensorflow/tensorflow/issues/30427), 雖然是無害的警告, 但是對於觀察 script 執行會造成困擾, 升級到 tensorflow-2.0.0-rc1 可以解決. 如何[升到 tensorflow-2.0.0-rc1](https://www.tensorflow.org/install/pip) 呢?

	> pip install tf-nightly
	> pip install tensorflow==2.0.0-rc1


然後試驗一下:

	> python -c "import tensorflow as tf;print(tf.reduce_sum(tf.random.normal([1000, 1000])))"

便不再出現煩人的警告了!

後記: 如果從 TF source 編譯, 加上選項支援 AVX2, 那要另外裝 bazel 套件, TensorFlow 自從 1.16 以後就支援 AVX2 了. 所以直接安裝 2.0.0-rc1 是最快的選項.

以上是被惡搞一天的心得, 希望下次如果重裝時可以參考.
