Title: iOS bluetooth route observation
Date: 2016-09-14 11:00
Category: Ios
Tags: Bluetooth Route
Has_Math: True

當 iDevice 連結 A2DP 藍芽裝置的情況下, 呼叫 [AVAudioSession setActive:error:] 會改變 current route, 動作如下：

	- (BOOL)prepareAudioSession {

	    AVAudioSession *session = [AVAudioSession sharedInstance];
	    // deactivate session
	    BOOL success = [session setActive:NO error: nil];
	    if (!success) {
	        DLog(@"deactivationError");
	    }

	    NSArray *availOutputs = [[session currentRoute] outputs];
	    DLog(@"1. avail outputs: %@", availOutputs);

	    // set audio session category AVAudioSessionCategoryPlayAndRecord options AVAudioSessionCategoryOptionAllowBluetooth
	    success = [session setCategory:AVAudioSessionCategoryPlayAndRecord withOptions:AVAudioSessionCategoryOptionAllowBluetooth error:nil];

	    if (!success) {
	        DLog(@"setCategoryError");
	    }

	    availOutputs = [[session currentRoute] outputs];
	    DLog(@"2. avail outputs: %@", availOutputs);

	    // activate audio session
	    success = [session setActive:YES error: nil];
	    if (!success) {
	        DLog(@"activationError");
	    }


	    availOutputs = [[session currentRoute] outputs];
	    DLog(@"3. avail outputs: %@", availOutputs);

	    [self startObservingVolumeChange];
	    DLog(@"prepare Audio Session OK");
	    return success;
	}

我們分別在 3 個地方印出 current route, 得到 log:

	2016-09-13 22:12:12.289 Ryht[503:137748] -[AppDelegate prepareAudioSession] 1. avail outputs: (
	    "<AVAudioSessionPortDescription: 0x136e97920, type = BluetoothA2DPOutput; name = Ryet-0002; UID = FE:67:77:67:00:02-tacl; selectedDataSource = (null)>"
	)
	2016-09-13 22:12:12.292 Ryht[503:137748] -[AppDelegate prepareAudioSession] 2. avail outputs: (
	    "<AVAudioSessionPortDescription: 0x136e97920, type = BluetoothA2DPOutput; name = Ryet-0002; UID = FE:67:77:67:00:02-tacl; selectedDataSource = (null)>"
	)
	2016-09-13 22:12:12.529 Ryht[503:137748] -[AppDelegate prepareAudioSession] 3. avail outputs: (
	    "<AVAudioSessionPortDescription: 0x136e978f0, type = Receiver; name = \U63a5\U6536\U5668; UID = Built-In Receiver; selectedDataSource = (null)>"
	)
	2016-09-13 22:12:12.529 Ryht[503:137748] -[AppDelegate prepareAudioSession] prepare Audio Session OK


可以看到, 在第三個列印時, current route 就不同了#
