layout: post
title: "Blender rendering code trace(1)"
date: 2016-05-24 16:55:13 +0800
comments: true
Category: Blender
Tags: Study, Render
Has_Math: True

前言：這系列我現在還不確定要用幾個 blogs 才能追蹤完，因為我還沒有看完；但是怕看了後面忘記前面，只好先跑。

<!--More-->
##範例##


##幾個重要的 functions##
當我們選擇 `Cycle Render Engine`, View port shading 切到 `Rendered`, 此時, 會執行到 __Session::run()__, 就以此為起點.

__Session::run()__ 的重要的兩件就是 __device->load_kernels()__ 與 __run_cpu()__ 或 __run_gpu()__, devices 是指這些 devices:

	OpenCLDeviceBase
	OpenCLDeviceMegaKernel
	OpenCLDeviceSplitKernel
	MultiDevice
	CUDADevice

這些 classes 都衍生自 __Device__ class, 並實作 __Device::load_kernels()__， 它們的任務就是 compile shader program 並將結果存到 cache 或 寫到某個目錄下.

__device->load_kernels()__ 在上面每一個 Class 的實作都不同, 我們先拿 __OpenCLDeviceBase__ class 往下追蹤:


	:::c
	bool load_kernels(const DeviceRequestedFeatures& requested_features)
	{
	...
		/* Try to use cached kernel. */
		thread_scoped_lock cache_locker;
		cpProgram = load_cached_kernel(requested_features,
					       OpenCLCache::OCL_DEV_BASE_PROGRAM,
					       cache_locker);

		if(!cpProgram) {
		... 尋找一些 path

				/* If does not exist or loading binary failed, compile kernel. */
				if(!compile_kernel(kernel_path,
						   init_kernel_source,
						   build_flags,
						   &cpProgram,
						   debug_src))
				{
					return false;
				}

				/* Save binary for reuse. */
				if(!save_binary(&cpProgram, clbin)) {
					return false;
				}
			}

			/* Cache the program. */
			store_cached_kernel(cpPlatform,
					    cdDevice,
					    cpProgram,
					    OpenCLCache::OCL_DEV_BASE_PROGRAM,
					    cache_locker);
		}
		else {
			VLOG(2) << "Found cached OpenCL kernel.";
		}

		/* Find kernels. */
	#define FIND_KERNEL(kernel_var, kernel_name)				\
		do {								\
			kernel_var = clCreateKernel(cpProgram, "kernel_ocl_" kernel_name, &ciErr); \
			if(opencl_error(ciErr))					\
				return false;					\
		} while(0)

		FIND_KERNEL(ckFilmConvertByteKernel, "convert_to_byte");
		FIND_KERNEL(ckFilmConvertHalfFloatKernel, "convert_to_half_float");
		FIND_KERNEL(ckShaderKernel, "shader");
		FIND_KERNEL(ckBakeKernel, "bake");

	#undef FIND_KERNEL
		return true;
	}


流程大致上為 載入 cached kernel, 如果不存在的話, 就找出來並且 compile, 然後存到 cache
以下討論這些片段：

	1. _load cached kernel program_
	2. _compile kernel_
	3. _save result_
	4. _create kernel_

##1. load cached kernel program##

首先 __load_cached_kernel__(requested_features, OpenCLCache::OCL_DEV_BASE_PROGRAM, cache_locker);
會呼叫 __OpenCLCache::getProgram()__ 裡面是：

	:::c++

	...
	case OCL_DEV_BASE_PROGRAM:

		/* Get program related to OpenCLDeviceBase */
		program = get_something<cl_program>(
			platform, device, &Slot::ocl_dev_base_program, slot_locker);
		break;

	case OCL_DEV_MEGAKERNEL_PROGRAM:

		/* Get program related to megakernel */
		program = get_something<cl_program>(
			platform, device, &Slot::ocl_dev_megakernel_program, slot_locker);
		break;
	...



這倆種 case 差別只在第三個參數 __&Slot::ocl_dev_base_program__ 與 __&Slot::ocl_dev_megakernel_program__. 然後在 __get_something<cl_program>()__ 裡面利用

	:::c++

	pair<CacheMap::iterator,bool> ins = self.cache.insert(
			CacheMap::value_type(PlatformDevicePair(platform, device), Slot()));
	Slot &slot = ins.first->second;




做出一個 key-value pair, key 的部份又是由另一個 pair (platform, device) 組出來的, value 的部份是 __OpenCLCache__ 的一個 nested struct __Slot__:

	:::c++

	class OpenCLCache
	{
		struct Slot
		{
			thread_mutex *mutex;
			cl_context context;
			/* cl_program for shader, bake, film_convert kernels (used in OpenCLDeviceBase) */
			cl_program ocl_dev_base_program;
			/* cl_program for megakernel (used in OpenCLDeviceMegaKernel) */
			cl_program ocl_dev_megakernel_program;

			Slot() : mutex(NULL),
			         context(NULL),
			         ocl_dev_base_program(NULL),
			         ocl_dev_megakernel_program(NULL) {}
		...



__Slot()__ 的建構子, _ocl_dev_base_program_, _ocl_dev_megakernel_program_ 都初始化為 NULL, 亦即剛才的 value 就是建構這個物件.

至此, __load_cached_kernel__ 已完成. 接著 __compile_kernel()__:


##2.compile kernel##

以下分別就 _OpenCLDeviceBase::compile_kernel()_ 與  _CUDADevice::compile_kernel() 討論：

_OpenCLDeviceBase_ 這邊, 重點是呼叫 OpenCL API:

	:::c++

	*kernel_program = clCreateProgramWithSource(cxContext, 1, &source_str, &source_len, &ciErr);
	...
	if(!build_kernel(kernel_program, custom_kernel_build_options, debug_src))
			return false;


__build_kernel()__ 同樣也是呼叫 OpenCL API:

	:::c++

	bool build_kernel(cl_program *kernel_program,
	                  string custom_kernel_build_options,
	                  const string *debug_src = NULL)
	{
	...

	ciErr = clBuildProgram(*kernel_program, 0, NULL, build_options.c_str(), NULL, NULL);
	...
	}



_CUDADevice_ 是做出 compile command, 然後呼叫 system call: _system_ 執行 nvcc compilation:

	:::c++

			string command = string_printf("\"%s\" -arch=sm_%d%d -m%d --cubin \"%s\" "
			"-o \"%s\" --ptxas-options=\"-v\" --use_fast_math -I\"%s\" "
			"-DNVCC -D__KERNEL_CUDA_VERSION__=%d",
			nvcc, major, minor, machine, kernel.c_str(), cubin.c_str(), include.c_str(), cuda_version);

			...

			if(system(command.c_str()) == -1) {
			...
			}



##3.save result##

這個步驟是把 compile 過的 kernel program 存到剛才的 slot 物件的 member variable (_ocl_dev_base_program_ or _ocl_dev_megakernel_program_).

	:::c++

	template<typename T>
	static void store_something(cl_platform_id platform,
	                            cl_device_id device,
	                            T thing,
	                            T Slot::*member,
	                            thread_scoped_lock& slot_locker)
	{
		...

		thread_scoped_lock cache_lock(self.cache_lock);
		CacheMap::iterator i = self.cache.find(PlatformDevicePair(platform, device));
		cache_lock.unlock();

		Slot &slot = i->second;

		/* sanity check */
		assert(i != self.cache.end());
		assert(slot.*member == NULL);

		slot.*member = thing;

		/* unlock the slot */
		slot_locker.unlock();
	}



傳入的參數 _T thing_ 即為 compile 過的 program. _iterator i_ 指到先前所說的 key-value pair, 故 i->second 即為 建構的 Slot() 物件, 這裡就是把 cl_program object 記錄在 Slot() member var 裡面.

##4.create kernel##
最後一段是 Macro 展開：

	:::c++

		FIND_KERNEL(ckFilmConvertByteKernel, "convert_to_byte");
		FIND_KERNEL(ckFilmConvertHalfFloatKernel, "convert_to_half_float");
		FIND_KERNEL(ckShaderKernel, "shader");
		FIND_KERNEL(ckBakeKernel, "bake");



將分別展開為:

	:::c++

		ckFilmConvertByteKernel =
			clCreateKernel(cpProgram, kernel_ocl_convert_to_byte, &ciErr);
		ckFilmConvertHalfFloatKernel =
			clCreateKernel(cpProgram, kernel_ocl_convert_to_half_float, &ciErr);
		ckShaderKernel =
			clCreateKernel(cpProgram, kernel_ocl_shader, &ciErr);
		ckBakeKernel =
			clCreateKernel(cpProgram, kernel_ocl_bake, &ciErr);



這些都是在 kernel.cl 裡面的 shader programs.

好！今天就把 __OpenCLDeviceBase:load_kernels()__ 的流程過一遍了. 至於其他 devices:

	OpenCLDeviceMegaKernel
	OpenCLDeviceSplitKernel
	MultiDevice
	CUDADevice

就等以後有時間再接力追蹤了！
