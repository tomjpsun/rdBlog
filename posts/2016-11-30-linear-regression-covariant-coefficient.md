title: "Linear Regression Coefficient"
date: 2016-11-30 17:21:22 +0800
has_math: True
Category: Math
Tags: mathjax, Linear Regression
Has_Math: True

本篇紀錄 **推導線性迴歸參數** 的過程.

首先回顧一下, 線性迴歸的一些基本符號：

$$hat{Y_i} 是自變數 X_i 的估計值 :$$

$$ 令 \hat{Y_i} = \hat{\beta_0} + \hat{\beta_1}X_i $$

$$Y_i 為實際的觀察值:$$

![](/images/regression_line.png)

在統計學裡面, 關於線性迴歸的參數, 都看到直接給出 Covariant Coefficient 的結果:

$$ \hat{\beta_1} = \frac{\sum\limits_{i=1}^n (X_i - \bar{X})(Y_i - \bar{Y})} {\sum\limits_{i=1}^n \big[X_i^2 - \bar{X}^2 \big] } \tag 1 $$



這個式子如何得到的呢？ 從所有點的估計值與觀察值 差平方和 開始：

$$ Q = \sum\limits_{i=1}^n (Y_i - \hat{Y_i})^2 = \sum\limits_{i=1}^n \big( Y_i - (\hat{\beta_0} + \hat{\beta_1}X_i) \big)^2 $$

我們希望 Q 有最小值, 即：

$$ \frac{\partial Q}{\partial \hat{\beta_0}} = \sum\limits_{i=1}^n 2 \big[Y_i-(\hat{\beta_0} + \hat{\beta_1}X_i) \big] (-1) = 0 \tag 2 $$

and

$$ \frac{\partial Q}{\partial \hat{\beta_1}} = \sum\limits_{i=1}^n 2 \big[ Y_i - (\hat{\beta_0} + \hat{\beta_1}X_i) \big] (-X_i) = 0 \tag 3 $$

由 (2) 得到：

$$ \sum\limits_{i=1}^n Y_i = \sum\limits_{i=1}^n (\hat{\beta_0} + \hat{\beta_1}X_i) \\ = n \hat{\beta_0} + \hat{\beta_1} \sum\limits_{i=1}^n X_i \tag 4 $$

由 (3) 得到：

$$ \sum\limits_{i=1}^n Y_i X_i = \sum\limits_{i=1}^n (\hat{\beta_0} + \hat{\beta_1}X_i) X_i \\ = \hat{\beta_0}\sum\limits_{i=1}^n X_i + \hat{\beta_1}\sum\limits_{i=1}^n (X_i)^2 \tag 5 $$

由(4) 得到:

$$ \hat{\beta_0} = \frac{\sum\limits_{i=1}^n Y_i - \hat{\beta_1} \sum\limits_{i=1}^n X_i}{n}  $$

帶入 (5):

$$ \sum\limits_{i=1}^n Y_i X_i = \frac{\sum\limits_{i=1}^n Y_i - \hat{\beta_1} \sum\limits_{i=1}^n X_i}{n} \sum\limits_{i=1}^n X_i + \hat{\beta_1} \sum\limits_{i=1}^n (X_i)^2 \\ = (\frac{1}{n}) \sum\limits_{i=1}^n Y_i \sum\limits_{i=1}^n X_i - (\frac{1}{n})\hat{\beta_1}\sum\limits_{i=1}^n X_i \sum\limits_{i=1}^n X_i + \hat{\beta_1} \sum\limits_{i=1}^n(X_i)^2 $$

整理得到：

$$ \hat{\beta_1} = \frac{\sum\limits_{i=1}^n X_i Y_i - \frac{\sum\limits_{i=1}^n X_i \sum\limits_{i=1}^n Y_i}{n}}{\sum\limits_{i=1}^n X_i^2 - \frac{(\sum\limits_{i=1}^n X_i)^2}{n}}  = \frac{\sum\limits_{i=1}^n X_i Y_i - n \bar{X} \bar{Y}}{\sum\limits_{i=1}^n X_i^2 - n \bar{X}^2 } \tag 6$$

其中

$$ \bar{X} = \frac{ \sum\limits_{i=1}^n X_i}{n} \\ \bar{Y} = \frac{ \sum\limits_{i=1}^n Y_i}{n} $$

現在先來看看一個式子：

$$ \begin{align}
& \sum\limits_{i=1}^n (X_i - \bar{X})(Y_i - \bar{Y}) \nonumber \\
&= \sum\limits_{i=1}^n (X_i Y_i - \bar{X} Y_i - \bar{Y} X_i + \bar{X}\bar{Y}) \nonumber \\
&= \sum\limits_{i=1}^n X_i Y_i - \bar{X} \sum\limits_{i=1}^n Y_i - \bar{Y} \sum\limits_{i=1}^n X_i + n\bar{X}\bar{Y} \nonumber \\
&= \sum\limits_{i=1}^n X_i Y_i - n \bar{X} \bar{Y} - n \bar{X} \bar{Y} + n \bar{X} \bar{Y} \nonumber \\
&= \sum\limits_{i=1}^n X_i Y_i - n \bar{X} \bar{Y} \nonumber
\end{align}
$$

此即為(6)的分子部分.

再看另一個式子：

$$\begin{align}
& \sum\limits_{i=1}^n (X_i - \bar{X})^2 \nonumber \\
&= \sum\limits_{i=1}^n (X_i^2 - 2 X_i\bar{X} + \bar{X}^2) \nonumber \\
&= \sum\limits_{i=1}^n X_i^2 - 2 \bar{X} \sum\limits_{i=1}^n X_i + n\bar{X}^2 \nonumber \\
&= \sum\limits_{i=1}^n X_i^2 - 2 \bigg( \frac{\sum\limits_{i=1}^nX_i}{n} \bigg) \sum\limits_{i=1}^n X_i + n \bigg( \frac{\sum\limits_{i=1}^n X_i}{n} \bigg)^2 \nonumber \\
&= \sum\limits_{i=1}^n X_i^2 - \frac{2}{n} \bigg( \sum\limits_{i=1}^n X_i \bigg)^2 + \frac{1}{n} \bigg( \sum\limits_{i=1}^n X_i \bigg)^2 \nonumber \\
&= \sum\limits_{i=1}^n X_i^2 - \frac{1}{n} \bigg( \sum\limits_{i=1}^n X_i \bigg)^2 \nonumber \\
&= \sum\limits_{i=1}^n X_i^2 - n\bar{X}^2 \nonumber
\end{align}
$$

此即為(6)的分母部分.

故(6)式可以寫成：

$$ \hat{\beta_1} = \frac{\sum\limits_{i=1}^n (X_i - \bar{X})(Y_i - \bar{Y})}{\sum\limits_{i=1}^n (X_i - \bar{X})^2 } = \frac{S_{xy}}{S_{xx}}$$

參考資料：

[南台科技大學 統計學 ch 11](http://ocw.stust.edu.tw/Sysid/ocw/bussinss/statistics_10002/CH11.pdf)

[銘傳應用統計系 統計學 3](http://www.mcu.edu.tw/department/management/stat/ch_web/etea/Statistics-3-net/chap23.pdf)

[https://isites.harvard.edu/fs/docs/icb.topic515975.files/OLSDerivation.pdf](https://isites.harvard.edu/fs/docs/icb.topic515975.files/OLSDerivation.pdf)
