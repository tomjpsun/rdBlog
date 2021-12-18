title: "Engineering Math Note (10) - 面積分觀念
date: 2017-05-05 22:30:00 +0800
has_math: True
Category: Math
Tags: Vector Analysis
Has_Math: True

$$ \newcommand{\norm}[1]{\left\lVert#1\right\rVert} $$
$$ \newcommand{\fpart}[2]{\frac{\partial #1}{\partial #2}} $$


本篇記錄面積分的基本觀念.

面積分的條件裡面,常常會出現需要被積分的函數 F(x,y,z), 與沿著曲面 S 積分,

然後積分範圍又是透過另外一些 equations 標示出 S 的範圍. 在代入積分式的時候,

就會令人暈眩. 如果了解基本觀念, 就不會弄錯對象了.

本篇附圖均來自：

1. [Advanced Engineering Math 5th](https://www.amazon.com/Advanced-Engineering-Mathematics-Dennis-Zill/dp/1449691722/ref=sr_1_1?s=books&ie=UTF8&qid=1492852721&sr=1-1&keywords=advanced+engineering+mathematics+zill) by Zill.
2. [Thomas's Calculus: Early Transcendentals (12th edition)](https://www.amazon.com/Thomas-Calculus-Transcendentals-Single-Variable/dp/0321628837/ref=sr_1_1?s=books&ie=UTF8&qid=1493993970&sr=1-1&keywords=thomas+calculus+early+transcendentals+12th+edition)

本小節取材自 Thomas' Calculus 16.5: Surface And Area, 因為這個部分, 它解釋得比其他工數課本清楚.

###參數化：由二維空間對映到曲線###

<img src="http://coding-addict.com/pictures/rd/surface_area_uv_map_1.png" alt="Drawing" style="width: 680px;"/>

上圖左邊是參數 u,v 組成的二維空間, 比照曲線可以由一個 t 當參數, 那麼曲面 S 可以由 u,v 來當參數表示,
亦即從左邊 u,v 二維空間對映到右邊的曲面 S, 記為:

$S: \{r(u,v)\}$
$$\textbf r(u,v) = f(u,v)\textbf i + g(u,v)\textbf j + h(u,v)\textbf k \tag 1$$
其中
$x = f(u,v)\\
y = g(u,v)\\
z = h(u,v)$

又 $P_0 = r(u_0, v_0)$

左邊 $\Delta A_{uv}$ 對映到右邊 $\Delta\sigma_{uv}$

###導入切平面###

<img src="http://coding-addict.com/pictures/rd/surface_area_uv_map_2.png" alt="Drawing" style="width: 350px;"/>

將 S 參數化了之後, 此時如果固定 v, 只讓 u 變動, 令 $v = v_0$, 則我們會在曲面 S 上得到以 $u$ 為參數的曲線, 如上圖藍線.

$\textbf r_u$ 就是該曲線在 $P_0$ 上的切線, 其意義為:

$$\textbf r_u = \lim_{\Delta u \to 0} \frac{\textbf r(u_0 + \Delta u, v_0) - \textbf r(u_0, v_0)}{\Delta u} \tag 2$$

同理, 此時如果固定 u, 只讓 v 變動, 令 $u = u_0$, 則我們會得到上圖橘線曲線, 而且該曲線在 $P_0$ 上的切線是：

$$\textbf r_v = \lim_{\Delta v \to 0} \frac{\textbf r(u_0, v_0 + \Delta v) - \textbf r(u_0, v_0)}{\Delta v} \tag 3$$

如下圖, 我們想要以橘色切平面逼近小塊曲面 $\Delta \sigma_{uv}$:

<img src="http://coding-addict.com/pictures/rd/surface_area_uv_map_3.png" alt="Drawing" style="width: 350px;"/>

橘色切平面的面積為:

$$\left\vert \Delta u \textbf r_u \times \Delta v \textbf r_v \right\vert = \left\vert \textbf r_u \times \textbf r_v \right\vert \Delta u \Delta v$$

也就是：
$$\Delta \sigma_{uv} \approx \left\vert \textbf r_u \times \textbf r_v \right\vert \Delta u \Delta v$$

如果 $a<u<b,\;c<v<d$, 則有 Surface Area A:

$$A = \int_a^b\int_c^d \left\vert \textbf r_u \times \textbf r_v \right\vert\,du\,dv \tag 4$$

為了簡明, 我們記做:

$$A = \iint_R d\sigma$$

其中：

$$d\sigma =  \left\vert \textbf r_u \times \textbf r_v \right\vert\,du\,dv \tag 5$$

我們常常會遇到曲面 S 以 $z = f(x,y)$ 表示, 這時候 $\textbf r$ 就直接以 x, y 為參數：

$$ \textbf r(x,y) = x \textbf i + y \textbf j + h(x,y) \textbf k$$

$$ \textbf r_x = \textbf i + h_x \textbf k \\
   \textbf r_y = \textbf j + h_y \textbf k $$

$$ \textbf r_x \times \textbf r_y  = \begin{vmatrix} \textbf i & \textbf j & \textbf k \\ 1 & 0 & h_x \\ 0 & 1 & h_y \end{vmatrix} = -f_x \textbf i - f_y \textbf j + \textbf k $$

$$\left\vert \textbf r_x \times \textbf r_y \right\vert = \sqrt {h_x^2 + h_y^2 + 1}$$

$$d\sigma = \sqrt {h_x^2 + h_y^2 + 1}\,dx\,dy \tag 6$$

注意, 在這裡 x, y 即是代表 u, v 的參數, 所以 $xy$ 平面就是參數的二維空間, 我們可以透過某些方程式找到 x, y 的範圍, 從而進行積分.

###Implicit Surface###


有時候, 我們只有 $\,F(x,y,z) = c\,$, 無法得到 (1) 式的參數化結果. 沒關係, 在高等微積分裡面, 有一個定理叫 _Implicit Function Theorem_ ,
以下圖來解釋:

<img src="http://coding-addict.com/pictures/rd/surface_area_implicit_surface.png" alt="Drawing" style="width: 300px;"/>

圖中, S 是由 $F(x,y,z) = c$ 所定義的曲面， R 是 S 投影在某平面上的區域, $\textbf p$ 是 R 的單位法向量.

當 R 屬於 $xy$ 平面時, 我們有: $\textbf p = \textbf k$: $\textbf k$是 $z$ 軸單位向量.

我們雖然無法知道參數化的曲面, 也就是我們沒有 explicit representation of $h(x,y)$.

_Implicit Function Theorem_ 說, 當 $F_z \neq 0$ 時, S 就是某一個 implicit function: $z = h(x,y)$,

即便我們無法明確知道 $h(x,y)$ 的方程式.

接下來, 我們需要一個 implicit differentation 的關係式：

`Theorem: a Formula for Implicit Differentiation`:

Let $F(x,y,z) = c$ defines variable z implicitly as a function $z = h(x,y)$, then

$$F(x,y,z) = F(x,y,h(x,y)) = c$$

by chain rule:

$$\fpart{F}{x}\fpart{x}{x} + \fpart{F}{y}\fpart{y}{x} + \fpart{F}{z}\fpart{z}{x} = 0\\
F_x + 0 + F_z \fpart{z}{x} = 0$$

$$\fpart{z}{x} = - \frac{F_x}{F_z} \quad and\quad \fpart{z}{y} = - \frac{F_y}{F_z} \tag 7$$


有了(7)式之後, 我們可以導出 $\textbf r_u, \textbf r_v$ 與 $\nabla F$ 的關係:

$$\textbf r(u,v) = u\textbf i + v\textbf j + h(u,v)\textbf k $$

$$\textbf r_u = \textbf i + \fpart{h}{u} \textbf k \quad and \quad \textbf r_v = \textbf j + \fpart{h}{v} \textbf k$$

對照 (7) 式 $z=h,\,x=u,\,y=v$：

$$\textbf r_u = \textbf i + \fpart{z}{x} \textbf k \quad and \quad \textbf r_v = \textbf j + \fpart{z}{y} \textbf k$$

$$\textbf r_u = \textbf i - \frac{F_x}{F_z} \textbf k \quad and \quad \textbf r_v = \textbf j - \frac{F_y}{F_z} \textbf k$$

$$ \begin{align}
   \textbf r_u \times \textbf r_v
   &= \frac{F_x}{F_z}\textbf i + \frac{F_y}{F_z}\textbf j + \textbf k \notag\\
   &= \frac{1}{F_z}(F_x \textbf i + F_y \textbf j + F_z \textbf k) \notag\\
   &= \frac{\nabla F}{F_z} \notag\\
   &= \frac{\nabla F}{\nabla F \cdot \textbf k} \notag\\
   &= \frac{\nabla F}{\nabla F \cdot \textbf p} \tag 8
 \end{align}$$

$$d\sigma =  \left\vert \textbf r_u \times \textbf r_v \right\vert\,du\,dv = \frac{\left\vert\nabla F\right\vert}{\left\vert\nabla F \cdot \textbf p\right\vert} \tag 9$$

我們得到 (9) 式, 是利用 Implicit Function Theorem , 當沒有辦法對曲面找到參數時, 仍然可以求得曲面.

###Surface Integral: Definition###

給定一個函數 $G(x,y,z)$, 我們想要對它沿著曲面 S 積分, 就是 Surface Integral, 定義為：

$$\iint_S G(x,y,z) d\sigma$$

當我們用參數 $\{ u,v \,\vert\, u,v \in R \}$  表示 S, 可以這樣求得：

$$\iint_S G(x,y,z) \, d\sigma = \iint_R G(f(u,v), g(u,v), h(u,v)) \left\vert \textbf r_u \times \textbf r_v \right\vert \,du\,dv \tag{10}$$

當它是 implicit, 可以這樣求:

$$\iint_S G(x,y,z) \, d\sigma = \iint_R G(x,y,z) \frac{\left\vert\nabla F\right\vert}{\left\vert\nabla F \cdot \textbf p\right\vert} \,dx\,dy \tag{11}$$

當它是 explicitly as $z=h(x,y)$, 可以這樣求:

$$\iint_S G(x,y,z) \, d\sigma = \iint_R G(x,y,z) \sqrt{f_x^2+f_y^2+1}\,dx\,dy \tag{12}$$

###Flux###

物理上有一個很重要的應用, 計算力場作用在物體表面上的力, 或通過物體表面的流量, 如電通量, 就是一種 Surface Integral, 它定義 被積分的方程式 G 為：

$$\textbf F \cdot \textbf n$$

其中 $\textbf F = M(x,y,z)\textbf i + N(x,y,z) \textbf j + P(x,y,z) \textbf k$

因為實在太重要了, 我們直接定義它為:

$$Flux = \iint_S \textbf F \cdot \textbf n dS$$
