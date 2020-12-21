title: "Engineering Math Note (6) - vector functions
date: 2017-04-01 15:00:22 +0800
has_math: True
Category: Math
Tags: Vector Analysis
Has_Math: True

$$ \newcommand{\norm}[1]{\left\lVert#1\right\rVert} $$

本篇整理一下 Vector functions 的關係：

Let r be a vector defined by t:

$$\textbf r(t) = f(t)\textbf i + g(t) \textbf j + h(t) \textbf k $$

$\,\textbf v\,$ 為速度, 與 $\,\textbf r\,$的關係是 :

$$ \textbf v =\textbf r'(t) = \frac{d\textbf r}{dt} \tag 1$$

$\textbf T\,$ 代表單位切線向量, 與 $\,\textbf v\,$同方向：

$$ \textbf T(t) = \frac{\textbf r'(t)}{\norm{\textbf r'(t)}} = \frac{\textbf v}{\norm {\textbf v}} \tag{2} $$

單位法向量$\,\textbf N\,$ 垂直於切線:

$$\textbf N(t) = \frac{\textbf T'}{\norm{\textbf T'}} \tag 3$$

$\, \textbf r(a)\,$ 到 $\,\textbf r(b)\,$ 的路徑 s 定義為 :

$$s = \int_a^b \norm{\textbf r'(t)}dt \tag 4$$

(4)式積分可以得到 s 與 t 的關係, 將參數 t 轉換為參數s, 令其關係為 t(s), 則 r(t) 可以換成 r(s):

$$\textbf r(s) = \textbf r(t(s)) \tag 5$$

(4)式微分可得：

$$\frac{ds}{dt} = \norm{ \textbf r'(t) }$$

s 參數可以簡化許多式, 例如(2):

$$\textbf r'(s) = \frac{d\textbf r(s)}{ds} = \frac{d\textbf r(t(s))}{dt}\frac{dt}{ds} = \cfrac{\textbf r'(t)}{\frac{ds}{dt}} = \textbf N(t) = \textbf N(s) \tag 6$$

因為 $\textbf N$ 是單位向量, 用 t 或 s 當參數, 方向與量 都不變, 故上式是可以互通的.

Curvature $\, \kappa \,$ 定義為 單位切線變化對弧線變化率:

$$\kappa = \norm{\frac{d\textbf N}{ds}} =  \frac{\norm{\textbf N'(t)}}{\norm{\textbf r'(t)}} \tag 7$$

$\textbf v = v\textbf N\,$則加速度:
$$\textbf a = \frac{d\textbf v}{dt} = \frac {dv}{dt} \textbf N + v \frac{d\textbf N}{dt} \tag 8$$

上式就是 a 分解為 _法向量_ 與 _切線向量_ 的組合, 可以記做：

$$ \textbf a = a_N \textbf N + a_T \textbf T \tag 9$$

將 $\textbf a$ 與 $\textbf T$ 分別做內積與外積, 可以吧 $\, a_N \, , \, a_T \,$分離出來：

because $\textbf N \cdot \textbf T = 0$:

$$\textbf a \cdot \textbf T = a_T \textbf T \cdot \textbf T = a_T$$

$$ \Rightarrow a_T = \norm{\textbf a \cdot \textbf T} \tag {10}$$

because $\textbf T \times \textbf T = 0$
and
$\textbf N \times \textbf T = \textbf B$:

$$\textbf a \times \textbf T = a_N \textbf N \times \textbf T = a_N\textbf B$$

since $\norm{\textbf B} = 1$

$$ \Rightarrow a_N = \norm{a_N \textbf B} = \norm{\textbf a \times \textbf T} \tag {11}$$
#
