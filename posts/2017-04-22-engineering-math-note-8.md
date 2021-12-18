title: "Engineering Math Note (8) - 線積分
date: 2017-04-22 12:51:22 +0800
has_math: True
Category: Math
Tags: Vector Analysis
Has_Math: True

$$ \newcommand{\norm}[1]{\left\lVert#1\right\rVert} $$
$$ \newcommand{\fpart}[2]{\frac{\partial #1}{\partial #2}} $$

本篇記錄學習線積分的心得. 本篇附圖均來自 [Advanced Engineering Math 5th](https://www.amazon.com/Advanced-Engineering-Mathematics-Dennis-Zill/dp/1449691722/ref=sr_1_1?s=books&ie=UTF8&qid=1492852721&sr=1-1&keywords=advanced+engineering+mathematics+zill) by Zill

##線積分##

先複習以前 微積分 課程, 沿著x軸作積分的概念：

<img src="http://coding-addict.com/pictures/rd/integrate_along_x_axis.png" alt="Drawing" style="width: 200px;"/>

如上圖, 一個函數 f 沿著 x 軸從 a 積分到 b, 會切成許多段, $\Delta x = x_k - x_{k-1}$ , 令 $\, x_{k-1}$ 與 $x_k$ 的中點為 $x_{k}^* \,$,其積分表示式為：

$$\int_a^b f(x)dx = \lim_{\Delta x \to 0} \sum_{k=1}^n f(x_k^*)\Delta x_k$$

同理推展到平面上的 G(x,y) 函數,  __沿著某一弧線線作積分__, 假設 C 為該弧線：

<img src="http://coding-addict.com/pictures/rd/integrate_along_line_segment.png" alt="Drawing" style="width: 200px;"/>

a. 相對於 x 軸, 即以 x 為參數：

$$\int_C G(x,y)dx = \lim_{\Delta x \to 0} \sum_{k=1}^n G(x_k^*,y_k^*)\Delta x_k \tag 1$$

b. 相對於 y 軸, 即以 y 為參數:

$$\int_C G(x,y)dy = \lim_{\Delta y \to 0} \sum_{k=1}^n G(x_k^*,y_k^*)\Delta y_k \tag 2$$

c, 相對於 s 弧線, 即以 s 為參數:

$$\int_C G(x,y)ds = \lim_{\Delta s \to 0} \sum_{k=1}^n G(x_k^*,y_k^*)\Delta s_k \tag 3$$

用變數變換成 以 t 為參數, 就是 x = f(t), y = g(t), $a \leq t \leq b$,

則上列式子可改寫為:

$$ \int_C G(x,y) dx = \int_a^b G(f(t), g(t)) f'(t) dt $$

$$ \int_C G(x,y) dy = \int_a^b G(f(t), g(t)) g'(t) dt $$

$$ \int_C G(x,y) ds = \int_a^b G(f(t), g(t)) \sqrt{f'(t)^2 + g'(t)^2} dt $$

注意: (1),(2) 式有時為了方便, 可以同時寫在一起：

$$\int_C P(x,y)dx + \int_C Q(x,y)dy = \int_C Pdx + Qdy \tag 4$$

##物理觀點##

線積分以物理觀點來看, 呈現幾種意義:

####面積####

<img src="http://coding-addict.com/pictures/rd/line_integral_get_area.png" alt="Drawing" style="width: 400px;"/>

上圖, 如果 G 代表空間曲面, 也就是 G(x,y) = z, 則 G 沿著曲線 C 的積分得到 上圖(b) 圍起來的'籬笆'

####功####

假設 $\textbf F(x,y)$ 代表力場函數 (磁力,電力,重力...等等), 在點 (x,y) 的作用力為 (以 $\textbf i, \textbf j$  分量表示)：

$$ \textbf F(x,y) = P(x,y)\textbf i + Q(x,y) \textbf j \tag 5$$

今有一曲線 C:

$$\textbf r = x \textbf i + y \textbf j $$

$$ \Rightarrow d\textbf r = dx \textbf i + dy \textbf j$$

則 F 沿著 C 所作的功為:

$$ W = \int_C \textbf F \cdot d\textbf r \tag 6$$

繼續展開, 即得:

$$ W = \int_C P dx + Q dy $$

<img src="http://coding-addict.com/pictures/rd/line_integral_in_fluid.png" alt="Drawing" style="width: 400px;"/>

如圖, 力場 F 對線圈 C 沿著其形狀作環積分, 得到的值, 表示力場對線圈 C 所作的功.
