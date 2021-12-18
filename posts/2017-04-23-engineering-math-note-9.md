title: "Engineering Math Note (9) - 格林定律
date: 2017-04-23 00:30:00 +0800
has_math: True
Category: Math
Tags: Vector Analysis
Has_Math: True

$$ \newcommand{\norm}[1]{\left\lVert#1\right\rVert} $$
$$ \newcommand{\fpart}[2]{\cfrac{\partial #1}{\partial #2}} $$


本篇記錄學習 Green's Theorem 的心得. 本篇附圖均來自 [Advanced Engineering Math 5th](https://www.amazon.com/Advanced-Engineering-Mathematics-Dennis-Zill/dp/1449691722/ref=sr_1_1?s=books&ie=UTF8&qid=1492852721&sr=1-1&keywords=advanced+engineering+mathematics+zill) by Zill

Green's Theorem, 簡單說：就是將 環積分 與 面積分 互相變換, 在實際應用裡可以簡化計算難度, 選擇以容易的方式求解.

###Green's Theorem:###


假設 C 是簡單封閉曲線, 圍成一區域 R, 如果 $P, Q, \fpart{P}{y}, \fpart{Q}{x}$ 在 R 上是連續的, 則

$$\oint_C Pdx + Qdy = \iint_R \big( \fpart{Q}{x} - \fpart{P}{y} \big)dA \tag 1$$

例：求 $\oint_C (x^5 + 3y)dx + (2x - e^{y^3})dy$, C 為圓:$(x-1)^2+(y-5)^2 = 4$

解：

$P(x,y) = (x^5 + 3y),\,Q(x,y) = 2x - e^{y^3}$

$\Rightarrow \fpart{P}{y} = 3,\,\fpart{Q}{x} = 2$

利用 Green's Theorem 轉換為面積分

$$ \oint_C (x^5 + 3y)dx + (2x - e^{y^3})dy = \iint_R (2-3) dA = -\iint_R dA$$

故所求為 -1 x 圓面積 = $-4\pi$

在繼續之前, 我們先訂正負方向: 若封閉區域在積分方向的左邊, 則積分方向為 positive, 如下圖 c 方向為負：

<img src="http://coding-addict.com/pictures/rd/greens_theorem_integral_direction.png" alt="Drawing" style="width: 300px;"/>

###Area with holes###


<img src="http://coding-addict.com/pictures/rd/greens_theorem_region_with_holes.png" alt="Drawing" style="width: 300px;"/>

如上圖 (a), $C_1$ 沿著順時針方向作積分, $C_2$ 沿著逆時針方向作積分, 區域 R 可以拆成(b) 的 $R_1,R_2$ 區域,

$C_r$ 環繞 $R_2$, $C_b$ 環繞 $R_1$.

根據 Green's Theorem, 繞行 $R_1, R_2$ 的線積分可轉換為 $R_1, R_2$ 的面積分, 又 (b) 圖中的截線, 其線積分兩兩抵銷,

設使 $C=C_r+C_b$, 則:

$$\oint_C Pdx + Qdy =\oint_{C_r} Pdx + Qdy + \oint_{C_b} Pdx + Qdy \tag 2\\
= \iint_{R_1} \big(\fpart{Q}{x} - \fpart{P}{y} \big) dA + \iint_{R_2} \big(\fpart{Q}{x} - \fpart{P}{y} \big) dA \\
= \iint_{R} \big(\fpart{Q}{x} - \fpart{P}{y} \big) dA$$

故知 Green's Theorem 依然適用於有洞的區域.

###Green's Theorem Not Applicable###


當遇到 $P, Q, \fpart{P}{y}, \fpart{Q}{x}$ 在 R 上不是連續的時候, 不能適用 Green's Theorem,

例一: 求 $\oint_C \cfrac{-y}{x^2+y^2} dx  + \cfrac{x}{x^2+y^2} dy$ 沿著 $C_1,C_2,C_3,C_4$ 的線積分？

<img src="http://coding-addict.com/pictures/rd/greens_theorem_not_applicable.png" alt="Drawing" style="width: 300px;"/>

$\fpart{P}{y} = \cfrac{y^2-x^2}{(x^2+y^2)^2}$

$\fpart{Q}{x} = \cfrac{y^2-x^2}{(x^2+y^2)^2}$

這裡不能適用 Green's Theorem, 因為原點 x=0, y=0, 會使得 $P, Q, \fpart{P}{y}, \fpart{Q}{x}$ 分母為 0 而不連續.

但是如果可以避開原點, 即可適用之, 如下例.

例二：求 $\oint_C \cfrac{-y}{x^2+y^2} dx  + \cfrac{x}{x^2+y^2} dy$ 沿著 $C_1,C_2$ 的線積分？

<img src="http://coding-addict.com/pictures/rd/greens_theorem_applicable_bypass_origin.png" alt="Drawing" style="width: 300px;"/>

同上題但是 $P, Q, \fpart{P}{y}, \fpart{Q}{x}$ 在 R 內連續(有定義), 可以適用 Green's Theorem:

$$\oint_C \cfrac{-y}{x^2+y^2} dx  + \cfrac{x}{x^2+y^2} dy = \iint_R \fpart{Q}{x} - \fpart{P}{y} dA = \iint_R 0 \ dA = 0$$

###Interesting Cases###


當 $\fpart{Q}{x} = \fpart{P}{y}$ 時, 有趣的事情出現了:

因為這表示用 Green's Theorem 轉換後的面積分 會得到 0,

再由 (2) 式:

$$\oint_{C_1} Pdx + Qdy + \oint_{C_2} Pdx + Qdy = 0$$

$$\Rightarrow \oint_{C_1} Pdx + Qdy = \oint_{-C_2} Pdx + Qdy$$

也就是說: 在此特殊情形下, 沿著 $C_1$ 與 $-C_2$ 積分結果相同.


心得： circle $C_1$ 包含 origin 時, 只是不能用 Green's Theorem  轉換, 但還是可以沿著曲線積分.

挖一個洞 $C_2$, 排除 origin 之後, 就可以用 Green's Theorem 轉換了.

然後在 $\fpart{Q}{x} = \fpart{P}{y}$ 時, 沿著 $C_1$ 積分結果與沿著 $C_2$ 積分結果一樣.


再回來看 例一, 因為 $\fpart{Q}{x} = \fpart{P}{y}$,

故沿著 $C_1,C_2,C_3,C_4$ 積分與 沿著一個圓, 線積分結果相同.

我們取一個圓參數為 $x=\cos t,\,y=\sin t,\quad 0 \leq t \leq 2\pi$ 代入

$$\oint_C \cfrac{-y}{x^2+y^2} dx  + \cfrac{x}{x^2+y^2} dy\\
=\int_0^{2\pi}\big[-\sin t(-\sin t) + \cos t(\cos t)\big]dt\\
=\int_0^{2\pi}(\sin^2 t + \cos^2 t)dt\\
=\int_0^{2\pi}dt = 2\pi$$


注意：上例有包含 origin, 我們沒有對它做 Green's Theorem 轉換, 只是找另一個圓, 比較容易求線積分 如此而已.
