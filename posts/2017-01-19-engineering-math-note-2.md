title: "Engineering Math Note (2)"
date: 2017-01-25 17:21:22 +0800
has_math: True
Category: Math
Tags: Differential Equation
Has_Math: True

####1st Order Linear Differential Equations

形式：$y'(x) + P(x)y(x) = Q(x)$

當 $Q(x) = 0$ 稱為 "homogeneous"(齊性方程式)

當 $Q(x) \neq 0$ 稱為 "nonhomogeneous"(非齊性方程式)

$$ \frac{dy}{dx} + P(x)y(x) = Q(x) $$

$$	dy + \big[P(x)y-Q(x)\big]dx = 0 $$

令 $M(x,y) = P(x)y - Q(x), N(x,y)=1$

則 $\cfrac{\partial M}{\partial y} = P(x), \cfrac{\partial N}{\partial x} = 0$ 為非正合方程式

是否存在積分因子？

由前一篇 (7) 式得到：

$$ f(x) =  \cfrac{ \cfrac{\partial M}{\partial y} - \cfrac{\partial N}{\partial x}}{N} = P(x) $$

是 x only, 故存在積分因子 I(x)

由前一篇 (8) 式得到：

$$ I(x) = exp\big(\int P(x)dx \big) $$

這裡巧妙的地方就是 $I(x)'=I(x)P(x)$

將 $I(x)$ 乘上原本 ODE:

$$ Iy' + IPy = IQ $$

為正合, 也就是：

$$ (Iy)' = IQ $$

兩邊積分得：

$$ \boxed{ Iy = \int IQdx + C } \tag 1$$

####Bernoulli’s Equation:

工程應用常見的方程式, 形式為：$y' + P(x)y = Q(x)y^n$, 可以變換為 1st order linear DE

$$ y^{-n}y' + P(x)y^{1-n} = Q(x) \tag 2 $$

令 $u = y^{1-n}$ 則

$$\cfrac{du}{dx} = (1-n)y^{-n}\frac{dy}{dx} \tag 3 $$

代回(2):

$$ \frac{du}{dx} + (1-n)P(x)u = (1-n)Q(x) \tag 4 $$

是 1st order linear DE, 直接用(1)的結果：

積分因子

$$I = exp (\int (1-n)P(x)dx)$$

(4)式左邊即是 $(Iu)'$, 右邊是 $(1-n)Q(x)$, 兩邊積分：

$$ Iu =\int I(1-n)Q(x)dx + C $$

即可求 u, 再換回 y 即可解.


參考資料：

[鄭老師的工數(一)](https://itunes.apple.com/tw/itunes-u/gong-cheng-shu-xue-yi-engineering/id1052999202?l=zh&mt=10)
