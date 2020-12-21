layout: post
title: "自己做的爛 bug"
date: 2014-03-15 22:00:51 +0800
comments: true
Category: Ios
Tags: Debug
Has_Math: True

昨天惡搞一下午的爛 bug，在此記錄一下：
<!-- more -->
執行時出現：

	Terminating app due to uncaught exception
	'NSInvalidArgumentException', reason: 'Could not find a storyboard named 'Main.storyboard' in bundle NSBundle
	</Users/tom/Library/Application Support/iPhone Simulator/7.1/
	Applications/9C602865-FB7E-462F-8C9B-5B84718E3B3E/
	Configurator.app> (loaded)'

追查原因，crash 是因為 storyboard 找不到：

	0   CoreFoundation                      0x017f01e4 __exceptionPreprocess + 180
	1   libobjc.A.dylib                     0x0156f8e5 objc_exception_throw + 44
	2   UIKit                               0x00793400 -[UIStoryboard name] + 0
	3   Configurator                        0x0000426b -[PListViewer tableView:didSelectRowAtIndexPath:] + 171

程式碼長這樣：

{% codeblock lang: objc %}

- (void)tableView:(UITableView *)tableView
didSelectRowAtIndexPath:- (NSIndexPath *)indexPath
{
    UIStoryboard *storyboard = [UIStoryboard storyboardWithName:@"Main.storyboard"
    bundle:[NSBundle mainBundle]];

{% endcodeblock %}

原來不能有副檔名，好低級的 Bug ...
`Main.storyboard` 改成 `Main` 就過了！

#
