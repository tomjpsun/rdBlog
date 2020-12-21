title: "Engineering Math Note (5) - Laplace Transform"
date: 2017-03-03 15:00:22 +0800
has_math: True
Category: Math
Tags: Differential Equation, Laplace Trasform
Has_Math: True

本篇是 Zill-Advanced Engineering Mathmatics 5th, Chap 4 的筆記. 討論拉普拉斯轉換(Laplace Transform).

`Laplace Transform Definition:`

Let f be a function defined for t >= 0, then the integral:

$$\mathscr{L}\{f(x)\} = \int_{0}^{\infty} e^{-st}f(t)dt \tag 1$$

is said to be the __Laplace transform__ of f, provided the integral __converges__（收斂）.

`Exponential Order:`

假設 M > 0, T > 0

如果存在一個常數 c 使得對於所有的 t > T, $|f(t)| <= Me^{ct}$ 成立,

則 f(t) 稱為 __Exponential Order__.

`Theorem 4.1.2 - Existance of Laplace Transform:`

If f(t) is piecewise continuous on interval [0, $\infty$)

and of exponential order, then $\mathscr{L}\{f(t)\}$ exists for s > c.

（心得：因為在此條件下(1)式收斂）

`Theorem 4.2.2 - Transform of Deriviate:`

$$\boxed{ \mathscr{L}\{f'(t)\} = sF(s) - f(0) } \tag 2$$

$$\boxed{ \mathscr{L}\{f''(t)\} = s^2F(s) - sf(0) - f'(0) } \tag 3$$

$$\boxed{ \mathscr{L}\{f^{(n)}(t)\} = s^{n}F(s) - s^{n-1}f(0) - s^{n-2}f'(0) - ... - f^{n-1}(0) } \tag 4$$

`Theorem 4.3.1 -  First Translation Theorem:`

$$ \boxed{ \mathscr{L}\{e^{at}f(t)\} = F(s-a) } \tag5 $$

$$ i.e.: \mathscr{L}^{-1} \{F(s-a)\} = e^{at}f(t) $$

Example:

$$ \mathscr{L}\{e^{5t}t^3\} = \mathscr{L}\{t^3\}_{s->s-5} = \left.\frac{3!}{s^4}\right\vert_{s->s-5} = \frac{6}{(s-5)^4}$$

(心得：先遮掉 $e^{at}$, 然後 Transform: f(t) -> F(s), 最後再對 s 位移)

`Theorem 4.3.2 - Second Translation Theorem:`

If $F(s) = \mathscr{L}\{f(t)\}$ and a > 0, then

$$ \boxed{ \mathscr{L}\{f(t-a) \mathscr{U}(t-a)\} = e^{-as}F(s) } \tag 6$$

(心得: 先從給的 f(t-a) 找到 f(t), 然後 Transform: f(t) -> F(s), 然後將結果乘上 $e^{-as}$ )

Inverse Transform：

$$ \mathscr{L}^{-1}\{e^{-as}F(s)\} = f(t-a)\mathscr{U}(t-a) \tag 7$$

反轉換比較容易錯, 容易誤以為從 F(s) 反轉換所得的 f(t) 就是答案, 忘記要讓它再變成 f(t-a)

Example:

$$ \mathscr{L}^{-1}\bigg\{ \frac{1}{s-4} e^{-2t} \bigg\} = e^{4(t-2)}\mathscr{U}(t-2)  $$

先遮去 $e^{-2t}$, 然後反轉換得到 $e^{4t}$, 再代入 shift t-2, 最後記得乘上 $\mathscr{U}(t-a)$

`Theorem 4.4.1 - Deriviatives of Transform:`

If $F(s) = \mathscr{L}\{f(t)\}$ and n = 1,2,3,...,n then:

$$ \mathscr{L}\{t^n f(t)\} = (-1)^n \frac{d^n}{ds^n}F(s) \tag 8$$

例子：

$$ Theorem \, 4.3.1:\quad \mathscr{L} \{ t e^{3t} \} = \mathscr{L} \{ t \}_{s \rightarrow s-3} = \left.\frac{1}{s^2} \right\vert_{s \rightarrow s-3} = \frac{1}{(s-3)^2}$$

$$ Theorem \, 4.4.1:\quad \mathscr{L} \{ t e^{3t} \} = -\frac{d}{ds}\mathscr{L}\{e^{3t}\} = \frac{d}{ds}\frac{1}{s-3} = \frac{1}{(s-3)^2}$$


`Definition of Convolution:`

If f,g 是 t 的函數, 則它們的 convolution 也是 t 的函數, 定義為：

$$ f * g = \int_{0}^{t} f(\tau)g(t-\tau) d\tau \tag 9$$

`Theorem 4.4.2 - Convolution Theorem:`

If f(t) and g(t) are piecewise continuous on [0, oo) and of exponential order, then:

$$\mathscr{L}\{ f * g \} = \mathscr{L} \{ f \} \mathscr{L} \{ g \} = F(s)G(s) \tag{10} $$

翻譯： 若 f,g 的拉普拉斯轉換有意義, 則它們 convolution 後的拉普拉斯轉換 = 個別轉換後的乘積.



證明會利用 _積分域的等價變換_ 的概念, 如下圖:

<img src="http://coding-addict.com/pictures/rd/exchange_domain_of_integration.png" alt="Drawing" style="width: 300px;"/>

$\tau$ 從 0 積分到 t, t 從 0 積分到 $\infty$ 可以變換為： t 從 $\tau$ 積分到 $\infty$, $\tau$ 從 0 積分到 $\infty$, 表示為：

$$ \int_{0}^{\infty} \bigg[ \int_{0}^{t} d\tau \bigg] dt = \int_{0}^{\infty} \bigg[ \int_{\tau}^{\infty} dt \bigg] d\tau$$

詳細證明需參考課本 4.4.2 節

目前為止，已經看過:

- s 的平移 (第一平移定理 Theorem 4.3.1)
- t 的平移 (第二平移定理 Theorem 4.3.2)
- f(t) 微分後轉換 (Theorem 4.2.2)
- 轉換後 F(s) 的微分 (Theorem 4.4.1)

現在來看 f(t) 積分後的轉換:

`Transform of Integral`

如果 convolution 的 $g(t) = 1$ 那麼 $\mathscr{L}\{g(t)\}=\frac{1}{s}$, (9) 式變成：

$$  f * g = \int_{0}^{t} f(\tau) d\tau $$

(10) 式變成：

$$ \mathscr{L} \{ f * g \} = \mathscr{L} \bigg\{ \int_{0}^{t} f(\tau) d\tau \bigg\} = \frac{F(s)}{s} \tag{11}$$

這樣 Laplace Transform 的各項操作都齊備了，也成為 解微分方程 的好工具！
#
