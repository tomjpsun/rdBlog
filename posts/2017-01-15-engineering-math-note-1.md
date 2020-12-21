title: "Engineering Math Note (1)"
date: 2017-01-18- 17:21:22 +0800
has_math: True
Category: Math
Tags: Differential Equation
Has_Math: True

本篇記錄一些解 ODE 會遇到的, 計算時可以 __直接套用__ 的公式, 可省下冗長的推導, 避免錯誤.

####正合方程式 (Exact Equation):

$$ M(x,y)dx + N(x,y)dy = 0 \tag 1$$

就是存在一函數 $\phi$ , 滿足：

$$ M(x,y)dx + N(x,y)dy = d\phi = 0 $$

則稱此方程式為 一階正合 ODE, 由全微分定理知道：

$$ d\phi = \frac{\partial\phi}{\partial x}dx + \frac{\partial\phi}{\partial y}dy = 0 $$

故：

$$ M(x,y) =\frac{\partial\phi}{\partial x} $$
$$ N(x,y) =\frac{\partial\phi}{\partial y} $$

所以判斷正合的方法為：

$$\boxed{ \frac{\partial M}{\partial y} = \frac{\partial N}{\partial x} } \bigg(= \frac{\partial^2\phi}{\partial x \partial y} \bigg) \tag 2$$

####引入積分因子:

若存在積分因子 I(x,y) 使得

$$ IMdx + INdy = 0 \tag 3$$

為正合方程式, 則 正合判斷式：

$$ \frac{\partial(IM)}{\partial y} = \frac{\partial(IN)}{\partial x} $$

$$ I\frac{\partial M}{\partial y} + M \frac{\partial I}{\partial y} = I \frac {\partial N}{\partial x} + N \frac{\partial I}{\partial x} $$

$$ I\bigg(\frac{\partial M}{\partial y} - \frac{\partial N}{\partial x}\bigg) = N \frac{\partial I}{\partial x} - M \frac{\partial I}{\partial y} \tag 4 $$

若 $I$ 為純 x 函數, 不與 y 有關, 即 $I = I(x)$ 則 (4) 式為：

$$ I\bigg(\frac{\partial M}{\partial y} - \frac{\partial N}{\partial x}\bigg) = N \frac{\partial I}{\partial x} = N \frac{dI}{dx} \tag 5 $$

若 $I$ 為純 y 函數, 不與 x 有關, 即 $I = I(y)$ 則 (4) 式為：

$$ I\bigg(\frac{\partial M}{\partial y} - \frac{\partial N}{\partial x}\bigg) = - M \frac{\partial I}{\partial y} = M \frac{dI}{dy}\tag 6 $$

由 (5) 知:

$$ \cfrac{ \cfrac{\partial M}{\partial y} - \cfrac{\partial N}{\partial x}}{N} dx =  \frac{dI}{I} \tag 7 $$

let

$$ f(x) =  \cfrac{ \cfrac{\partial M}{\partial y} - \cfrac{\partial N}{\partial x}}{N} $$

兩邊積分得：

$$ \int\frac{dI}{I} = \ln I = \int f(x)dx $$

故得：

$$ \boxed{ I = exp(\int f(x) dx) } \tag 8 $$

同理 let

$$ f(y) = -\cfrac{\cfrac{\partial M}{\partial y} - \cfrac{\partial N}{\partial x}}{M}$$

可得：

$$ \boxed{ I = exp(\int f(y) dy) } \tag 9 $$

這些推導的結果(框起來的公式), 將來可以直接代入, 不需每次推導了！

得到 $I$ 以後, 如何解呢？

將 $I$ 乘回 ODE:

$$ IMdx + INdy = d\phi = 0 $$

一樣，比較 $\phi$ 的全微分形式, 得到：

$$ IM = \frac{\partial \phi}{\partial x} = \frac{d\phi}{dx}$$
$$ \phi = \int IMdx + g(y) $$

$$ IN = \frac{\partial \phi}{\partial y} = \frac{d\phi}{dy}$$
$$ \phi = \int INdy + h(x) $$

取兩式的交集即可得到 $\phi$



參考資料：

[鄭老師的工數(一)](https://itunes.apple.com/tw/itunes-u/gong-cheng-shu-xue-yi-engineering/id1052999202?l=zh&mt=10)
