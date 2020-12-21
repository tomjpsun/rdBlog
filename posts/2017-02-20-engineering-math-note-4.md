title: "Engineering Math Note (4)"
date: 2017-02-20 17:21:22 +0800
has_math: True
Category: Math
Tags: Differential Equation
Has_Math: True

本篇記錄 n 階線性 ODE 的分類與解法.

`分類:`

微分方程式 $a_n(x)y^{(n)} + a_{n-1}(x)y^{(n-1)} + ... + a_1(x)y' + a_0y = g(x)$

如果 $a_{n}(x)$ 為常數: $a_{n}$， 叫n階常係數 線性ODE.

如果 $a_{n}(x)$ 為 x function，叫n階變係數 線性ODE

g(x)=0 叫 __齊性__

g(x)!=0 叫 __非齊性__

`二階常係數線性ODE：`

$$ a_{2}y''+a_{1}y'+a_{0}y = g(x) \tag 1$$

解題步驟：

(1)先求 homogeneous solution: $\,y_{h}$

令 $y_{h}=e^{mx}$ -> 特徵方程式: $a_{2}m^{2} + a_{1}m + a_{0} = 0$

- 如果 m 有兩相異實數根 $\Rightarrow  y_{h}=c_1 e^{m_1 x} + c_2 e^{m_2 x}$

- 如果 m 有兩重根 $\Rightarrow       y_{h}=c_1 e^{m_1 x} + c_2 xe^{m_1 x}$

- 如果 m 為兩虛根 $\Rightarrow       \alpha + \beta i, \alpha - \beta i$ -> $y_h = e^{\alpha x} (c_1 \cos\beta x + c_2 \sin\beta x)$

- note: 如果已知一解, 可以令 $y_2(x) = y_1(x)u(x) \Rightarrow$

$$y_2 = y_1 \int \cfrac{e^{- \int P(x) dx}}{y_1 ^2} \tag 2 $$

(2)再求 perticular solution: $y_p$

- (2-1) 對照 g(x) 表格: 課本 table 3.4.1, 造出 perticular solution

- (2-2) 用參數變異法 (常係數、變係數通用):

令 $\,y_p = u_1 y_1 + u_2 y_2$

其中 $y_1,\,y_2$ 是 homogeneous solutions

將 (1) 式整理為：

$$ y''+ P(x)y'+ Q(x)y = k(x) \tag 3$$

$$
\begin{cases}
y_1 u_1' + y_2 u_2' = 0  \\
y_1'u_1' + y_2'u_2' = k(x)  \\
\end{cases} \tag 4
$$

令

$$ w = \begin{vmatrix} y_1 & y_2 \\ y_1' & y_2' \end{vmatrix} $$

$$ w_1 = \begin{vmatrix} 0 & y_2 \\ k(x) & y_2' \end{vmatrix} $$

$$ w_2 = \begin{vmatrix} y_1 & 0 \\ y_1' & k(x) \end{vmatrix} $$

則 (4) 的解為(Textbook section 3.5 有推導過程)

$$ u_1' = \frac{w_1}{w}, \quad u_2' = \frac{w_2}{w} \tag 5 $$

`二階變係數線性ODE：`

一般都用 infinite series 來找解, 到 Chap.5 會介紹, 這裡先介紹一個特別的型態：_Cauchy-Eular Equation:_

$$ a_n X^n \frac{d^n y}{dx^n} + a_{n-1}x^{n-1}\frac{d^{n-1}y}{dx^{n-1}} + ... + a_1x \frac{dy}{dx} + a_0y = g(x) $$

本式特徵為：每一項微 n 次恰好與 X 的次方補齊, 最後每一項都是 $x^n$,

如果是二次, 其特徵方程式為：

$$ am^2 + (b-a)m + c = 0 $$

- m 若為兩相異實根, 解為 $ y=c_1 x^{m_1} + c_2 x^{m_2}$
- m 若為重根, 解為 $y=c_1 x^{m_1} + c_2 x^{m_1} lnx$
- m 若為兩虛根, 解為 $y = x^{\alpha} (c_1\cos\beta x + c_2 \sin\beta x)$
