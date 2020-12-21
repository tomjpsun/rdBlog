title: "Linear Operator on Inner Product Spaces"
date: 2019-03-12 06:40:00 +0800
has_math: True
Category: Math
Tags: Linear Algebra
Has_Math: True

$$ \newcommand{\inner}[2] {\langle {#1},{#2} \rangle} $$

[定理 10-5](#Theorem-10-5)
令 $\phi$ 為有限維內積空間 $V$ 上的線性泛函 (linear functional)
$\phi:V\mapsto K$,
則存在唯一 $v \in V \ni \phi (u) = \inner{u}{v} \ \forall \,u \in V$

[白話] 對於 從 $V$ 到 $K$ 的線性泛函 $\phi$, 以及 $V$ 內任意向量 $u$, 我們可以找到一個 $v$
使得 $u,v$ 內積, 等同於此線性泛函. i.e. $\phi(u)=\inner{u}{v}$

證明：
令 $\{u_1, u_2,...,u_n\}$ 為 $V$ 上的一組 orthonormal basis,
則可以將任意 $u$ 表示為
$$ u=a_1u_1+a_2u_2+ \ldots +a_nu_n \tag 1 $$

利用線性泛函的定義：

$$ \begin{aligned} \phi(u) &= \phi(a_1u_1+a_2u_2+...+a_nu_n) \\
                           &=  a_1\phi(u_1) + a_2\phi(u_2) + \ldots + a_n\phi(u_n) \\
\end{aligned} \tag 2 $$

以及內積的定義：

$$ \begin{aligned} \inner{u}{v} &= \inner{a_1u_1+ \ldots +a_nu_n}{v} \\
                         &= a_1\inner{u_1}{v} + \ldots + a_n\inner{u_n}{v} \\
						 \end{aligned} \tag 3 $$

比較(2)、(3) 式, 只要 $\phi(u_i)$ 等於 $<u_i,v>$
就有辦法了, 令

$$ v = \phi(u_1)u_1+\phi(u_2)u_2+ \ldots +\phi(u_n)u_n \tag 4 $$

就可以讓 (2)(3) 式相等

接下來證明唯一, 令 $v'$ 也是使得 $\phi(u)=\inner{u}{v'}$, 則 :

$\inner{u}{v}=\inner{u}{v'}$

$\inner{u}{v-v'}=0$, 對於任何 $u \in V$

故 $v=v'$ 得證 #

[定理10-6](#Theorem-10-6)
令 $T$ 為有限維內積空間$V$上的線性算子, 則存在唯一另一個線性算子 $T^*$ 使得

$$\inner{T(u)}{v} =\inner{u}{T^*(v)}, \,\forall \,u,v \in V  \tag 5$$

證明：

如果 $T$ 已經確定, 令 $\{u_1,u_2,...,u_n\}$ 為 $V$ 的一組 orthonormal basis  則

$$ \begin{aligned} &\inner{T(u_1)}{v}=\inner{u_1}{T^*(v)} \\
                   &\inner{T(u_2)}{v}=\inner{u_2}{T^*(v)} \\
	               & ... \\
                   &\inner{T(u_n)}{v}=\inner{u_n}{T^*(v)} \\
				   \end{aligned} \tag 6 $$

(6)式可以解讀為 $T^*(v)$ 在 $u_i$ 上的投影分量為 $\inner{T(u_i)}{v}, \, \forall i = 1 \ldots n$

所以我們根據 $T$ 可以定出 $T^*$:

$$ T^*(v) = \sum _{i=1 \ldots n}  \inner{T(u_i)}{v} u_i \tag 7 $$

故 $T^*$ 存在, 這樣子的定義很容易看出唯一性, 故得證 #

我們稱 $T^*$ 為 $T$ 的伴隨算子 (adjoint operator)

心得：在某組正交基底 $B$ 下, $T$ 的矩陣表示式為 $[T]^B_B$ 令之為 $[a_{ij}] = P$

又令 $[T^*]^B_B$ = $[b_{ij}] = Q$

對照定理10-3：


$$ \begin{aligned} a_{ij} &= \inner{T(u_j)}{u_i} \\
                   b_{ij} &= \inner{T^*(u_j)}{u_i} \\
	                      &= \overline{\inner{u_i}{T^*(u_j)}} \; \dots T's \; adjoint \\
				          &= \overline{\inner{T(u_i)}{u_j}} \\
						  &= \overline{a_{ji}} \\
\end{aligned}  $$

哇！伴隨算子的矩陣表示，竟然就是原算子的矩陣的

共軛轉置(Conjugate Transpose Matrix) i.e. $Q = P^*$


[定理 10-7](#Theorem-10-7) $S,T \in L: V \mapsto V , k \in K$:

(1) $(S+T)^* = S^* + T^*$

(2) $(kT)^* = kT^*$

(3) $(ST)^* = T^*S^*$

(4) $(T^*)^* = T$

證明 (1)：

$$ \inner{(S+T)(u)}{v} = \inner{u}{(S+T)^*(v)} \; 同時 \\
\begin{aligned} & \inner{(S+T)(u)}{v} = \inner{S(u)+T(u)}{v} \\
                &= \inner{S(u)}{v} + \inner{T(u)}{v} \\
				&= \inner{u}{S^*(v)} + \inner{u}{T^*(v)} \\
				&= \inner{u}{S^*(v)+T^*(v)} \\
				&= \inner {u}{(S+T)^*(v)} \\
\end{aligned}
$$
所以 $(S+T)^* = S^* + T^*$
(2)(3)(4) 比照(1) 的方式可證


[定理 10-8](#Theorem-10-8) Let $\lambda$ 為 $T$ 的特徵值

(1) 若$T^*=T^{-1}\ 則 \ \lambda = 1$

(2) 若$T^*=T\ 則 \ \lambda \in R$

(3) 若$T^*= -T\ 則 \ \lambda$ 為純虛數

(4) 若 $T=S^*S$, 其中 $S$ 為非奇異, 則 $\lambda > 0$
