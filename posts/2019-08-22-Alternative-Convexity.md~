Title: "Alternative characterization of convexity"
date: 2019-08-22 13:40:00 +0800
has_math: True
Category: Math
Tags: Linear Algebra
Has_Math: True

$$ \newcommand{\inner}[2] {\langle {#1},{#2} \rangle} \
\newcommand{\norm}[1] {\|{#1}\|} \
\newcommand{\normalize}[1] {\frac{#1}{\|{#1}\|}}
\newcommand{\vec} {\textbf}
$$

本文內容來自 [Optimization Model](https://www.amazon.com/Optimization-Models-Giuseppe-C-Calafiore-ebook-dp-B00LB6BB1O/dp/B00LB6BB1O/ref=mt_kindle?_encoding=UTF8&me=&qid=) 8.2.2 小節, 因為很瑣碎, 所以把這個部分整理、筆記一下.

這節談到一些相關於 convexity 的其他特性, 從 $f$ 是凸函數(convex function) 開始.

<!-- more -->


#凸函數(Convex function) 定義:#

$$ f:\mathbb R^n \mapsto \mathbb R, for \ all \ \lambda \in [0,1], \vec x, \vec y \in \mathbb R^n\\
f(\lambda \textbf x + (1-\lambda) \textbf y) \leq \lambda f(\textbf x) + (1-\lambda)f(\textbf y)\\ \tag{1}
$$

白話敘述就是一個上凹的形狀(曲線或曲面)

![](/images/convex_func_definition.png)

Maxima 繪出本圖指令：wxplot2d([x^2 + 3, 2*x+15], [x, -4, 6], [y, -2, 40], [legend, "f(x)", "λ ratio"], [label,["f(x_1)", -3, 10],["f(x_2)",4, 25], ["λf(x_1)+(1-λ)f(x_2)", 1, 17], ["f(λx_1+(1-λ)x_2)", 1, 3]]);

#First-order conditions#

若 $f$ 是 convex function, 那就滿足一個特性, 表示式為：

$$ f(\textbf x) + \nabla f(\textbf x)^T(\textbf v - \textbf x) \leq f(\textbf v) \tag{2}
$$

![](/images/convex_first_order_condition.png)

Maxima 繪出本圖指令 : wxplot2d([2*x^2+2, 4*x],[x, -3, 3], [legend, ""],[label, ["f(x) + Grad.f(x)^T(v-x)", -2, -10], ["(x,f(x))", 1, 3], ["f(v)", -2, 10]]);

白話的敘述就是, 如果 f 是 convex, 那麼它在任一點的切線或切面, 都可以是 f 的最大下線.


#Second-order conditions#

若 $f$ 的二次微分 (Hessian Matrix of f) 為正值, 則 f 是 convex function.

先看泰勒展開式: $f(\vec v)$ 在 $\vec x_0$ 的展開為：

$$ f(\vec v) = f(\vec x_0) + \nabla f(\vec x_0)^T (\vec v - \vec x) +
\frac {1}{2} (\vec v - \vec x)^T \nabla^2 f(\vec x_0) (\vec v - \vec x) \tag{3}
$$

其中第三項的 $\nabla^2 f$ 就是 f 的 Hessian matrix, 如果它是半正定矩陣, 表示第三項必為正值, 所以：

$$ f(\vec v) \geq f(\vec x_0) + \nabla f(\vec x_0)^T (\vec v - \vec x)
$$

再根據 (2) 式, 可知 f 為 convex function.


#Pointwise supremum (or maximum)#

Pointwise supremum 的維基定義是[這樣的](https://proofwiki.org/wiki/Definition:Pointwise_Supremum).

重新強調維基的重點:

$$ \bigg ( \underset{i \in I}{sup} f_i \bigg )(s) = \underset{ i \in I }{sup} \bigg( f_i(s) \bigg) \tag{4}
$$

數學語句看了蛋疼, 白話是：一堆函數先映射之後, 其上確界(supremum, 亦即最小上界), 等於先取這些函數的上確界函數, 再做映射的結果.

	觀念 1: 一個有序集合的上確界, 不一定在集合內, 但是極大值, 一定在集合內. 例如負實數的集合, 不存在有極大值,
	     因為對集合內任一個值, 該值除以 2 永遠比該值大. 但是此集合的上確界存在, 其值為 0.

	觀念 2: 一個集合如果是 compact (i.e.,closed and bounded) 那麼他的極大值就存在, 即為上確界.(max = sup)
	觀念 3: 大多數情況,可以把下列的 sup() 先以 max() 代入驗證

本定理白話是說: __如果一堆凸函數, 取它們的上確界函數, 也是得到一個凸函數.__


輔助定理: $sup ( f_1 + f_2, f_3 + f_4 ) \leq sup(f_1, f_3) + sup(f_2, f_4)$

因為

$f_1 \leq sup(f_1, f_3)$

$f_2 \leq sup(f_2, f_4)$

$f_1 + f_2 \leq sup(f_1, f_3) + sup(f_2, f_4)$

同理:

$f_3 + f_4 \leq sup(f_1, f_3) + sup(f_2, f_4)$

故可知：

$sup(f_1+f_2, f_3,+f_4) \leq sup(f_1,f_3) + sup(f_2,f_4) \tag {5}$


本定理證明: 先證明兩個. 令 $f_1$, $f_2$ 為兩個凸函數, 對任意 $x,y \in domf_1 \cap domf_2$

令 $f(x) = sup ( f_1(x), f_2(x) )$  則:

$$f(\lambda x + (1 - \lambda)y)
	= sup \big( f_1(\lambda x + (1 - \lambda)y), f_2(\lambda x + (1 - \lambda)y) \big) \tag{by (4)}\\
    = \big( sup(f_1, f_2) \big)(\lambda x + (1 - \lambda y)) \tag{6} \\
	&\leq sup(f_1(\lambda x) , f_2(\lambda x)) + sup(f_1( (1-\lambda)y) , f_2( (1 - \lambda)y)) \tag{by(5)} \\
	= \lambda sup(f_1,f_2)(x) + (1 - \lambda)sup(f_1,f_2)(y)$$

由此得證.

本命題可以一般化, 推至多個 functions 也適用.


#反向命題: Every closed convex function $f$ can be expressed as the pointwise supremum of affine functions.#

就是一個閉凸函數 $\bar f$ 就是許多 affine functions 的 supremum. 表示為:

$$\bar f(x) = sup \{ a(x): \space a \space  is \space affine, \space and \space a(z) \leq f(z), \forall z \} \tag{8}$$

![](/images/convex_is_affines_supremum.png)

[comment]: <> ( Maxima : wxplot2d([2*x^2+2, 4*x, 2*x + 3/2, -4*x, 8*x-6],[x, -4, 6], [legend, "f(x)", "a_1","a_2","a_3","a_4"]); )

[Stack Exchange 證明](https://math.stackexchange.com/questions/1960891/convex-function-can-be-written-as-supremum-of-some-affine-functions/1960927)


#Jensen's inequality for convex function#

令 $f:\mathbb R_n \mapsto \mathbb R$ 為凸函數, $\mathbb E$ 為期望值, $z \in dom f$,  則有：

$$ f(\mathbb E \{z\}) \leq \mathbb E \{f(z)\} \tag{9}
$$

由(8), f 為 supremum of affine functions. 令 $A$ 是所有在 $f$ 之下的 affine functions 的集合,
我們可以寫為：

$$ f(z) = \underset{ a \in A}{sup} \ a(z) \geq a(z), \forall a \in A
$$

上式兩邊取 $\mathbb E$, 記得 $\mathbb E$ 為單調遞增(monotone)

Note: 連續型定義看得出是 monotone:

$$\mathbb E(Y) = \int_{t=0}^{\infty} \mathbb P(Y > t)dt
$$

$$\mathbb E\{f(z)\} \geq \mathbb E\{a(z)\}, \forall a \in A
$$

亦即上式對所有的 $a(z)$ 都成立, 那麼一定大於它們的 sup :

$$\mathbb E\{f(z)\} \geq \underset{a \in A}{sup} \bigg( \mathbb E\{a(z)\} \bigg)
$$


因為 a 是 affine, 而且 $\mathbb E\{\}$ 是線性運算子, 因此

$$\mathbb E\{f(z)\} \geq \underset{a \in A}{sup} \ a(\mathbb E\{z\}) = f(\mathbb E\{z\}) \tag{10}
$$

QED

#Jensen's inequality for concave functions#

是上面的相關命題, f is convex => -f is concave, 不等式需要顛倒

$$ \mathbb E \{f(z)\} \tag{11}  \leq f(\mathbb E \{z\})
$$



Jensen's inequality for concave functions 可以用來證明 am-gm 不等式:

給定一堆正數 $x_1, ... x_n$ 它們的 gm 是：

$$ f_g(\vec x) = \bigg( \prod_{i=1}^n x_i \bigg)^{\frac{1}{n}}
$$

它們的 am 是:

$$ f_a(\vec x) = \frac{1}{n} \sum_{i=1}^{n}x_i
$$

am-gm 不等式是說:

$$f_g(x) \leq f_a(x)
$$

證明：兩邊取 log 得, log 是 concaved function:

$$
\begin{align}
	log \ f_g(\vec x) &= \frac{1}{n}\sum_{i=1}^{n}log\ (x_i) \\
	          &\leq log \bigg( \frac{\sum_{i=1}^{n} x_i}{n} \bigg) \tag{ by (11)}\\
			  &= logf_a(\vec x)
\end{align}
$$

由於 log 是 monotone, 故得證 $f_g(\vec x) \leq f_a(\vec x)$
