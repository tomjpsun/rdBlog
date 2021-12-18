title: "Engineering Math Note (7) - Directional Derivative
date: 2017-04-07 15:00:22 +0800
has_math: True
Category: Math
Tags: Vector Analysis
Has_Math: True

$$ \newcommand{\norm}[1]{\left\lVert#1\right\rVert} $$
$$ \newcommand{\fpart}[2]{\frac{\partial #1}{\partial #2}} $$


本篇記錄方向導數的推導.

以前微積分對於導數的定義是：

$$ \lim_{\Delta x \to 0} \frac{f(x+\Delta x) - f(x)}{\Delta x} $$

可以解釋為對 x 方向上微分, 那麼對任意方向$\,\vec {PQ}\,$微分呢？就是方向導數的概念,

推導之前先定義一些東西：

<img src="http://coding-addict.com/pictures/rd/directional derivative with s.png" alt="Drawing" style="width: 400px;"/>

Note: 上圖是參考自 Kreyszig: "Advanced Engineering Mathematics fig.188"

如上圖所示, $\vec {PQ}$ 是我們要做導數的方向向量, $\,\textbf b \,$ 是該方向的單位向量, s 為 $\vec {PQ}$ 的弧長, 則

L 上任意一點以 s 為參數表示時就會是：

$$ r(s)=x(s) \textbf i + y(s) \textbf j + z(s) \textbf k = P + s \textbf b \tag 1$$

我們定義函數 f(x,y,z) 在 P 沿著單位向量 $\textbf b$ 的方向導數為:

$$ D_b f = \frac{df}{ds} = \lim_{s \to 0} \cfrac{f(Q)-f(P)}{s} \tag 2$$

由 chain rule 知:

$$D_b f = \cfrac{df}{ds} = \fpart{f}{x}x' + \fpart{f}{y}y' + \fpart{f}{z}z' \tag 3$$

$r'=x'\textbf i + y'\textbf j + z'\textbf k = \textbf b\,$ ,(3) 繼續推導即得:

$$D_b f = \nabla f \cdot r' = \nabla f \cdot \textbf b \tag 4$$


####另外一種證法####

<img src="http://coding-addict.com/pictures/rd/directional derivative diagram.png" alt="Drawing" style="width: 500px;"/>

Note: 上圖是參考自 Zill: "Advanced Engineering Mathematics Fig-9.5.2"

P 是 $\,f(x_0,y_0)\,$, f 為曲面函數(藍色部分),

有一個包含 $\overline {PQ}$ 且與 x-y 平面垂直的平面(橘色平面), C 是由該平面與 曲面交集形成的曲線,

h 是 $\overline {PQ}$ 投影在 x-y 平面上的線段, $\textbf u$ 是 $\vec {PQ}$ 在 x-y 平面上投影向量的單位向量,

定義 f 沿著 $\vec {PQ}$ 方向的導數為:

$$D_u f = \lim_{h \to 0} \frac{f(x_0 + \Delta x, y_0 + \Delta y) - f(x_0, y_0)}{h} \tag 5$$

關鍵就是令一個函數 g:

$$ g(h) = f(x_0 + \Delta x, y_0 + \Delta y) = f(x_0 + hcos\theta, y_0 + hsin\theta) \tag 6 $$

其中：

$$ x = x_0 + h cos \theta $$

$$ y = y_0 + h sin \theta \tag 7$$

g 對 h 微分剛好就是我們要推導的 (5) 式：

$$ g'(h) = \lim_{h \to 0} \frac{g(h) - g(0)}{h} = \lim_{h \to 0} \frac{f(x_0 + \Delta x, y_0 + \Delta y) - f(x_0, y_0)}{h}  \tag 8$$

by Chain rule:

$$ \Rightarrow g'(h) = f'(h) = \fpart{f}{x} \frac{dx}{dh} + \fpart{f}{y} \frac{dy}{dh} \\
	= \fpart{f}{x} cos \theta + \fpart{f}{y} \sin \theta \qquad by (7) \\
	= \nabla f \cdot \textbf u$$

第一種證明 比較優雅, 但比較抽象； 第二種證明 比較貼近直觀, 但是繪圖較複雜.
