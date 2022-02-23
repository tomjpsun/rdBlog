title: Install CUDA-toolkit-11.1 For TensorFlow 2.0
slug: install_cuda_toolkit_for_tensorflow
date: 2021-01-24 12:33:45 UTC+08:00
type: text

因為比較健忘,在此記錄一下程序：

1. 到 NVIDIA 官網下載 CUDA toolkit, 目前提供到 11.1 and 11.2, 我們採用 11.1, 因為 libcudnn 目前搭配 CUDA 11.1,
   google search 'CUDA toolkit download' 即可找到

2. 依據自己系統選擇適當的 installer, 這裡我用 Debian 10, .deb file, 此檔案打包了所有 cuda*.deb 的套件:
![](/images/select_cuda_toolkit_download.png)

3. 依照步騶安裝：

		sudo dpkg -i cuda-repo-debian10-11-1-local_11.1.0-455.23.05-1_amd64.deb
		sudo add-apt-repository contrib
		sudo apt-get update
		sudo apt-get -y install cuda

4. CUDA DNN lib 需另外安裝, 也是到官網下載, [VIDIA cuDNN download:](https://developer.nvidia.com/CUDNN), 選項很多, 因為 Debian 與 Ubunbu 接近, 選用 cuDNN Runtime Library for Ubuntu20.04 x86_64 (Deb)

		sudo dpkg -i [cuDNN lib].deb

檢測, 跑下面的 neuron.py 程式： [後記]

		from tensorflow import keras
		from tensorflow.keras import layers
		#Create a network with 1 linear unit
		model = keras.Sequential([
			layers.Dense(units=1, input_shape=[3])
		])


得到 console print：

	(ENV) tom@atlantic:~/work/PythonProj/kaggle-dl-course/sec-1$ python neuron.py
	2021-01-24 13:01:45.741577: I tensorflow/stream_executor/platform/default/dso_loader.cc:49] Successfully opened dynamic library libcudart.so.11.0
	2021-01-24 13:01:46.470510: I tensorflow/compiler/jit/xla_cpu_device.cc:41] Not creating XLA devices, tf_xla_enable_xla_devices not set
	2021-01-24 13:01:46.471063: I tensorflow/stream_executor/platform/default/dso_loader.cc:49] Successfully opened dynamic library libcuda.so.1
	2021-01-24 13:01:46.511482: I tensorflow/stream_executor/cuda/cuda_gpu_executor.cc:941] successful NUMA node read from SysFS had negative value (-1), but there must be at least one NUMA node, so returning NUMA node zero
	2021-01-24 13:01:46.511858: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1720] Found device 0 with properties:
	pciBusID: 0000:01:00.0 name: GeForce RTX 2080 Ti computeCapability: 7.5
	coreClock: 1.545GHz coreCount: 68 deviceMemorySize: 10.76GiB deviceMemoryBandwidth: 573.69GiB/s
	2021-01-24 13:01:46.511888: I tensorflow/stream_executor/platform/default/dso_loader.cc:49] Successfully opened dynamic library libcudart.so.11.0
	2021-01-24 13:01:46.513657: I tensorflow/stream_executor/platform/default/dso_loader.cc:49] Successfully opened dynamic library libcublas.so.11
	2021-01-24 13:01:46.513687: I tensorflow/stream_executor/platform/default/dso_loader.cc:49] Successfully opened dynamic library libcublasLt.so.11
	2021-01-24 13:01:46.514330: I tensorflow/stream_executor/platform/default/dso_loader.cc:49] Successfully opened dynamic library libcufft.so.10
	2021-01-24 13:01:46.514498: I tensorflow/stream_executor/platform/default/dso_loader.cc:49] Successfully opened dynamic library libcurand.so.10
	2021-01-24 13:01:46.515752: I tensorflow/stream_executor/platform/default/dso_loader.cc:49] Successfully opened dynamic library libcusolver.so.10
	2021-01-24 13:01:46.516177: I tensorflow/stream_executor/platform/default/dso_loader.cc:49] Successfully opened dynamic library libcusparse.so.11
	2021-01-24 13:01:46.516268: I tensorflow/stream_executor/platform/default/dso_loader.cc:49] Successfully opened dynamic library libcudnn.so.8
	2021-01-24 13:01:46.516345: I tensorflow/stream_executor/cuda/cuda_gpu_executor.cc:941] successful NUMA node read from SysFS had negative value (-1), but there must be at least one NUMA node, so returning NUMA node zero
	2021-01-24 13:01:46.516872: I tensorflow/stream_executor/cuda/cuda_gpu_executor.cc:941] successful NUMA node read from SysFS had negative value (-1), but there must be at least one NUMA node, so returning NUMA node zero
	2021-01-24 13:01:46.517209: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1862] Adding visible gpu devices: 0
	2021-01-24 13:01:46.517884: I tensorflow/compiler/jit/xla_gpu_device.cc:99] Not creating XLA devices, tf_xla_enable_xla_devices not set
	2021-01-24 13:01:46.517976: I tensorflow/stream_executor/cuda/cuda_gpu_executor.cc:941] successful NUMA node read from SysFS had negative value (-1), but there must be at least one NUMA node, so returning NUMA node zero
	2021-01-24 13:01:46.518310: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1720] Found device 0 with properties:
	pciBusID: 0000:01:00.0 name: GeForce RTX 2080 Ti computeCapability: 7.5
	coreClock: 1.545GHz coreCount: 68 deviceMemorySize: 10.76GiB deviceMemoryBandwidth: 573.69GiB/s
	2021-01-24 13:01:46.518324: I tensorflow/stream_executor/platform/default/dso_loader.cc:49] Successfully opened dynamic library libcudart.so.11.0
	2021-01-24 13:01:46.518335: I tensorflow/stream_executor/platform/default/dso_loader.cc:49] Successfully opened dynamic library libcublas.so.11
	2021-01-24 13:01:46.518343: I tensorflow/stream_executor/platform/default/dso_loader.cc:49] Successfully opened dynamic library libcublasLt.so.11
	2021-01-24 13:01:46.518351: I tensorflow/stream_executor/platform/default/dso_loader.cc:49] Successfully opened dynamic library libcufft.so.10
	2021-01-24 13:01:46.518359: I tensorflow/stream_executor/platform/default/dso_loader.cc:49] Successfully opened dynamic library libcurand.so.10
	2021-01-24 13:01:46.518367: I tensorflow/stream_executor/platform/default/dso_loader.cc:49] Successfully opened dynamic library libcusolver.so.10
	2021-01-24 13:01:46.518375: I tensorflow/stream_executor/platform/default/dso_loader.cc:49] Successfully opened dynamic library libcusparse.so.11
	2021-01-24 13:01:46.518383: I tensorflow/stream_executor/platform/default/dso_loader.cc:49] Successfully opened dynamic library libcudnn.so.8
	2021-01-24 13:01:46.518425: I tensorflow/stream_executor/cuda/cuda_gpu_executor.cc:941] successful NUMA node read from SysFS had negative value (-1), but there must be at least one NUMA node, so returning NUMA node zero
	2021-01-24 13:01:46.518808: I tensorflow/stream_executor/cuda/cuda_gpu_executor.cc:941] successful NUMA node read from SysFS had negative value (-1), but there must be at least one NUMA node, so returning NUMA node zero
	2021-01-24 13:01:46.519123: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1862] Adding visible gpu devices: 0
	2021-01-24 13:01:46.519142: I tensorflow/stream_executor/platform/default/dso_loader.cc:49] Successfully opened dynamic library libcudart.so.11.0
	2021-01-24 13:01:46.904378: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1261] Device interconnect StreamExecutor with strength 1 edge matrix:
	2021-01-24 13:01:46.904405: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1267]      0
	2021-01-24 13:01:46.904411: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1280] 0:   N
	2021-01-24 13:01:46.904544: I tensorflow/stream_executor/cuda/cuda_gpu_executor.cc:941] successful NUMA node read from SysFS had negative value (-1), but there must be at least one NUMA node, so returning NUMA node zero
	2021-01-24 13:01:46.904906: I tensorflow/stream_executor/cuda/cuda_gpu_executor.cc:941] successful NUMA node read from SysFS had negative value (-1), but there must be at least one NUMA node, so returning NUMA node zero
	2021-01-24 13:01:46.905241: I tensorflow/stream_executor/cuda/cuda_gpu_executor.cc:941] successful NUMA node read from SysFS had negative value (-1), but there must be at least one NUMA node, so returning NUMA node zero
	2021-01-24 13:01:46.905553: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1406] Created TensorFlow device (/job:localhost/replica:0/task:0/device:GPU:0 with 9794 MB memory) -> physical GPU (device: 0, name: GeForce RTX 2080 Ti, pci bus id: 0000:01:00.0, compute capability: 7.5)
	(ENV) tom@atlantic:~/work/PythonProj/kaggle-dl-course/sec-1$


其中 ##Successfully opened dynamic library libcu*## 都成功, 表示安裝 OK


[後記]
目前 cuda-11-1 相容版本為

	tensorflow             2.4

NVIDIA 監控工具： nvtop, nvidia-smi
