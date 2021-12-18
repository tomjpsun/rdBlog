title: "iOS Certificate Problems"
date: 2013-08-13 16:52
comments: true
Category: Ios
Tags: Certificates
Has_Math: True

##The Certificate Expires …##

重做一個 Certificate 。

先解釋名詞：

Certificate 是把你的 public key，送到 iOS Developer Site 簽署（再綁一個 site 給的 key）後的東東。有了它， iOS Developer Site 可以認證你的身分。

<!-- more -->

Provision Profile 是綁許多人的 Certificates 產生的東東，通常是一個開發小組成員的 Certificates。若想要下載 iOS App 到機器上跑，都需要驗證一份 Provision Profile，以決定你的下載權利。

重要的前置動作！先把 鑰匙圈 App 裡面，過期的 Certificate 刪掉，從 |選單|顯示方式| 可以顯示過期的憑證， Xcode 對於重複的 key 會報錯誤訊息：code sign error.

鑰匙圈 App | 選單 | 鑰匙圈存取 | 憑證輔助程式 | 從憑證授權要求憑證…|

一般名稱：將來在鑰匙圈 App 裡面可以看到這個鑰匙的名字。CA 電子郵件 空白，選擇『貯存到磁碟』，然後先存到桌面 xxx.cer (檔名不重要)。

登入 iOS Developer site, 製作一個 Certificate，餵給它桌面上的 xxx.cer，之後 Developer site 就會產生一個新的 Certificate，下載後點兩下，會自動裝到 鑰匙圈 App。

註：製作 Development Certificate 與 Distribution Certificate 時可以使用同一個 xxx.cer，如果你懶得再做一個 cer file 的話，可以趁進入 iOS Developer site 時一次製作兩個。


##當進入 | xcode | organizer | devices | provisioning profiles | 右下角 refresh | 動作時, 發生 “Cannot find Distribution profile …there are pending certificate requests …” 的錯誤；但是在 iOS developer site 上面，卻沒有 pending 的 Certificate Request !##

Xcode 在 refresh 時會自動檢查 Development Profile and Distribution Profile . 通常這個現象，是因為在 iOS Developer Site 上只做了 Development Profile 而還沒有 Distribution Profile.

解決方法很簡單，就是到 iOS Developer Site 製作 Distribution Profile. 讓 Xcode 在 refresh 時可以同時找到這兩種 profile.

[中文 key ring 參考資料](http://www.minwt.com/ios/4411.html)

