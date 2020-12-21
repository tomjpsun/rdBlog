title: "Orthonormal Basis"
date: 2019-04-04 08:07:00 +0800
has_math: True
Category: Math
Tags: Linear Algebra
Has_Math: True

$$ \newcommand{\inner}[2] {\langle {#1},{#2} \rangle} $$

本章介紹線性代數很重要的定理，白話一些就是介紹 “線性映射” 在某一組正交基底下 所對應的 “矩陣表示式”.
<!-- more -->

[定理 10-3](#Theorem-10-3)
令 $B= \{ u_1,u_2,\dots,u_n \}$ 為 $V$ 的一組正交單位化基底 (O.N.B) 則:
$$ \begin{aligned} & (1) \forall v \in V, v = \inner{v}{u_1}u_1 + \inner{v}{u_2}u_2 + \dots + \inner{v}{u_n}u_n \\
                   & (2) 若 T \in L(V,V), 則 \big[ T \big]^B_B = \big[ a_{ij} \big]_{nxn} \quad 其中 a_{ij}=\inner{T(u_j)}{u_i} \\
\end{aligned}$$


證明： (1) 就是將 $v$ 表示為 在每個分量 $u_i$ 上的投影量, 可以設 $v=a_1u_1 + a_2u_2 + \dots + a_nu_n$
      讓 $v$ 與 $u_i ( \forall i = 1\dots n )$ 內積, 即可得到
$$	  \inner{v}{u_i} = a_i, \forall i = 1\dots n $$
得證

(2) 由(1) 知道
$$ \begin{aligned} \\
& T(u_1) = \inner{T(u_1)}{u_1}u_1 + \inner{T(u_1)}{u_2}u_2 + \dots + \inner{T(u_1)}{u_n}u_n \\
& T(u_2) = \inner{T(u_2)}{u_1}u_1 + \inner{T(u_2)}{u_2}u_2 + \dots + \inner{T(u_2)}{u_n}u_n \\
& \vdots \\
& T(u_n) = \inner{T(u_n)}{u_1}u_1 + \inner{T(u_n)}{u_2}u_2 + \dots + \inner{T(u_n)}{u_n}u_n \\
\end{aligned}$$
這是 $T$ 在 $B$ 下的 matrix representation:

$$    \big[ T \big]^B_B = \big[ a_{ij} \big]_{nxn} \quad  a_{ij} =\inner{T(u_j)}{u_i} $$
得證.



心得：這裡的條件 正交基底很重要，將來，我們利用這個定理可以知道，

某個線性映射 T，與其伴隨算子 都有矩陣表示式可以表示之。
