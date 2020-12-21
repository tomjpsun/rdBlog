title: "Matrix decomposition notes"
date: 2019-08-02 13:40:00 +0800
Category: Math
Tags: Linear Algebra
Has_Math: True

本文內容來自 [Mayer 的著作: Matrix Analysis and Applied Linear Algebra](https://www.amazon.com/Matrix-analysis-applied-linear-algebra/dp/0898714540)

##[定理] Orthogonal Decomposition Theorem##
任意 $A \in R_{m \times n}$:

$$ R(A)^{\perp} = N(A^T), \quad and \quad N(A)^{\perp} = R(A^T)
$$

##URV 分解##

對任意矩陣$A_{m \times n}$ 如果 $Rank(A) = r$, 可以做 URV Factorization.
使得:

.. math::

\begin{align}
A = URV^T = U
	\begin{bmatrix}
		\begin{array}{c|c}
			C_{r \times r} & O \\
			\hline
			O & O
		\end{array}
	\end{bmatrix}
	V^T
\end{align}


其中 $U$ 的行向量是 orthogonal basis of $R(A) \cup N(A^T)$,

$V$ 的行向量是 orthogonal bases of $R(A^T) \cup N(A)$
