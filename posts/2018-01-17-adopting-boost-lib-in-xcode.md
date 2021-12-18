title: "Adopting Boost Lib in Xcode"
date: 2018-01-17 20:56:00 +0800
Category: C++
Tags: Boost
Has_Math: True


在 Xcode 下使用 boost, 以下用範例程式 `main.cpp` 說明，比較清楚：

	1    #include <iostream>
	2    #include <boost/filesystem.hpp>
	3    #include <boost/system/error_code.hpp>
	4
	5
	6    using std::cout;
	7    using std::endl;
	8    using std::string;
	9    using namespace boost::filesystem;
	10   using namespace boost::system;
	11
	12   int main()
	13   {
	14    	cout << "Current path is : " << current_path().string() << endl;
	15   }
	
這裡引用了兩個 boost lib: <filesystem> 與 <system>,
所以 Xcode project 就要加入 "Add other..." library
	
<img src="http://coding-addict.com/pictures/rd/adopting_boost_in_xcode_add_lib.png" alt="Drawing" style="width: 800px;"/>

如圖按 `+` 後, 按下左下 `Add other` 按鈕, 按 `Cmd + Shift + g` 後出現路徑輸入,
然後打 `/usr/local/lib` 然後找到 `libboost_system.dylib` `libboost_filesystem.dylib` 加入即可.



	
