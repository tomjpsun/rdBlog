title: "Householder transformation notes"
date: 2019-06-27 13:40:00 +0800
Category: Math
Tags: Linear Algebra
Has_Math: True

$$ \newcommand{\inner}[2] {\langle {#1},{#2} \rangle} \
\newcommand{\norm}[1] {\|{#1}\|} \
\newcommand{\normalize}[1] {\frac{#1}{\|{#1}\|}} $$

本文內容來自 [Mayer 的著作: Matrix Analysis and Applied Linear Algebra](https://www.amazon.com/Matrix-analysis-applied-linear-algebra/dp/0898714540)

#單式矩陣 Unitary Matrix#

Unitary Matrix (單式矩陣) $U_{n \times n}$ 的行向量 $\in \mathbb{C}^n$ 的 orthonormal basis

Orthogonal Matrix (正交矩陣) $A_{n \times n}$ 的行向量 $\in \mathbb{R}^n$ orthonormal basis

故知道正交矩陣都是單式矩陣，具有以下單式矩陣的特性：

$[U^*U]_{i,j} = \delta(i,j)$

$U^*U = I$

Unitary 保模長, 所以 $x \in \mathbb{R}^n$ 有：

$\| Ux\|^2 = \inner{Ux}{Ux} = (Ux)^*Ux = x^*U^*Ux = x^*x =  \| x\|^2$

保內積, 所以:

$\inner{Ux}{Uy} = (Uy)^*(Ux) = y^*U^*Ux = y^*x = \inner{x}{y}$

#Elementary Orthogonal Projector#

對於某個單位向量 $u \in \mathbb{C}^n, \|u\| = 1$, 那麼對任意 $x \in \mathbb{C}^n$ 我們可以定義 Projector $Q$：

$$Q = I - uu^* \tag 1 $$

為將 $x$ 投影到 $u^\perp$ 的算子(Operator), 為什麼呢, 請看下圖：

![](/images/project_operator.png)

圖中, $(uu^*)x = u(u^*x) = \inner{x}{u}u$ 即為 $x$ 投影到 $u$ 的分量.

故 $x-uu^*x = (I-uu^*)x$ 即為 $x$ 投影到 $u^\perp$ 的分量

#Elementary Reflector#

給定任意向量 $u \in \mathbb{C}^n, \hat u = \normalize{u}$, 我們可以定義 Elementary Reflector $R$:

$$ R = I - 2 \hat u \hat u^* = I - 2 \frac{uu^*}{u^*u} = I - 2 \frac{uu^*}{\norm{u}^2} \tag 2 $$

請看下圖理解：

![](/images/reflect_operator.png)

若 $\mathbb{C}^n$ 的 basis 為 $\{e_1,e_2,...,e_n\}$, 對某個 basis $e_k,k \in 1...n$

我們想要將任意 $x$ 用 reflector 投影到該 basis, 那麼如何找出那個 Reflect operator 呢？

先 normalize $x$, 令 $\hat x = \normalize{x}$

那麼 $\hat x, e_k$ 形成菱形的兩個相鄰邊(都是 1 單位長), 如下圖:

![](/images/find_orthogonal_u.png)


由於菱形是對稱的, $\hat x + e_k$ 可以當鏡射線, 將 $\hat x$ 鏡射到 $e_k$,

又 $\hat x + e_k$ 會與我們要找的 $u$ 垂直, 因為菱形的對角線就是互相垂直, 很明顯的取

$$u = \hat x - e_k \tag 3$$

即可找到 $u$

令 $\hat x_k$ 為 $x$ 的第 k 個座標值, 我們很容易驗證:

$$u^*u = 2(u^*\hat x) \tag 4 $$

$$\begin{align}
u^*u &= (\hat x^*-e_k^*)(\hat x - e_k) \tag {by(3)} \\
     &= \hat x^*\hat x - e_k^*\hat x - \hat x^*e_k + e_k^*e_k \\
	 &= 1 - \hat x_k - \hat x_k + 1 \\
	 &= 2(1 - \hat x_k)	 \\
	 \\
u^*\hat x &= \hat x^*\hat x - e_k^*\hat x \\
	 &= 1 - \hat x_k
\end{align} $$

由(2)

$$\begin{align}
Rx &= \norm{x}R\hat x  \quad \quad \tag {by (2)} \\
   &= \norm{x}(\hat x - 2\frac{uu^*\hat x}{u^*u}) \quad \quad \quad (u^* \hat x \; is \; scalar) \\
   &= \norm{x}(\hat x - 2\frac{u^*\hat x}{u^*u}u)  \tag {by (4)} \\
   &= \norm{x}(\hat x - u) \tag {by (3)} \\
   &= \norm{x}e_k \tag 5 \
\end{align}
$$

果然可以將 $x$ 鏡像映射到 $e_k$

整理一下, 給定 $x,e_k$ 就可以定出 Elementary Reflector 將 $x$ 映射到 $e_k$:

$$ R = I - 2 \frac{uu^*}{\norm{u}^2} $$

其中 $u = \normalize{x} - e_k$

#Orthogonal Reduction (Householder Reduction)#

由 Elementary Matrix 的定義, 我們知道 $R$ 也是一種 elementary matrix,

因為 $R$ 是鏡射, $R$ 作用兩次就等於還原：

$$RR = I \implies R = R^{-1}$$

$$det(RR) = det(R)det(R) =  det(I) = 1 $$

$$det(R) =  \pm 1
\tag 6 $$

對於 $A \in \mathbb{C}^{n \times n} = \big [ A_{*,1} \big | A_{*,2} \big | ... \big | A_{*,n} \big ]$

取:

$$u_1 = \normalize {A_{*,1}} - e_1 $$

我們得到:

$$R_1 = I_n - 2\frac {u_1u_1^*}{u_1^*u_1}$$

使得：

$$R_1A_{*,1} =
\begin{bmatrix}
    t_{11} \\
    0 \\
    \vdots \\
    0
\end{bmatrix}
\tag 7 \
$$

由(5)知：

$$t_{11} = \norm{A_{*,1}} \tag 8 $$

Apply $R_1$ to $A$ yields:

$$ \begin{align}
R_1A &= R_1 \big [ A_{*,1} \big | A_{*,2} \big | ... \big | A_{*,n} \big ] \\
	&= \left[
	   \begin{array}{c|c}
		   t_{11} & {t_{12} \dots t_{1n}} \\
		   \hline
		   {0 \\ \vdots \\ 0 } & { * \dots * \\ \vdots \\ * \dots * }
	   \end{array}
	   \right] \\
	&= \left[
		\begin{array}{c|c}
		t_{11} & t_1^t \\
		\hline
		O & A^{(2)}
		\end{array}
		\right]
		\tag 9 \
\end{align}$$

$A^{(2)} \in \mathbb{C}^{(n-1) \times (n-1)}$

我們可以找到

$$ R_2 =
\left[
\begin{array}{c|c}
1 & O \\
\hline
O & \hat R_2
\end{array}
\right]
\tag {10} \
$$

which

$$\hat R_2 = I_{n-1} - 2\frac {u_2u_2^*}{u_2^*u_2}$$

這樣
$$R_2R_1A=
\left[
\begin{array}{c|c|c}
t_{11} & * & \cdots \\
\hline
{0 \\ \vdots\\ 0}  & { t_{22} \\ 0 \\ \vdots } & { \quad \\ A^{(3)} }
\end{array}
\right] \
\tag {11} $$

依序下去, 我們得到 __Orthogonal Reduction__ :

For:$ A \in \mathbb{C}^{n \times n}$, there exists a unitary matrix $P$ :

$$PA=T$$

$T$ is an upper triangular matrix, $P = R_n \cdots R_2 R_1$ is an Unitary matrix

(根據(10) 很容易驗證 $R_2R_2^{-1} = I \implies R_iR_i^{-1} = I, i = 1 \cdots n \implies PP^{-1} = I$)

值得注意的是, 由(8)可以看到, 每次找到的 $t_{11}, t_{22}, \cdots , t_{nn}$ 都是模長, 也就是 > 0
