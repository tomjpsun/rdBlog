layout: post
title: "Quick Inverse Matrix (4x4)"
date: 2016-05-25 17:21:22 +0800
comments: true
has_math: True
Category: Math
Tags: Matrix
Has_Math: True


今天來解開[Affine transformation](https://en.wikipedia.org/wiki/Affine_transformation#Properties)的面紗.

<!--More-->

程式碼是 Blender (Sources/cycles_util/Header Files/util_transform.h) 擷取來的:

The code snippet:


	:::c++
	 1 ccl_device_inline Transform transform_quick_inverse(Transform M)
	 2 	{
	 3 		/* possible optimization: can we avoid doing this altogether and construct
	 4 		 * the inverse matrix directly from negated translation, transposed rotation,
	 5 		 * scale can be inverted but what about shearing? */
	 6 		Transform R;
	 7 		float det = M.x.x*(M.z.z*M.y.y - M.z.y*M.y.z) -
	 8 		M.y.x*(M.z.z*M.x.y - M.z.y*M.x.z) + M.z.x*(M.y.z*M.x.y - M.y.y*M.x.z);
	 9
	10 		if(det == 0.0f) {
	11 			M.x.x += 1e-8f;
	12 			M.y.y += 1e-8f;
	13 			M.z.z += 1e-8f;
	14 			det = M.x.x*(M.z.z*M.y.y - M.z.y*M.y.z) -
	15 			M.y.x*(M.z.z*M.x.y - M.z.y*M.x.z) + M.z.x*(M.y.z*M.x.y - M.y.y*M.x.z);
	16 		}
	17 		det = (det != 0.0f)? 1.0f/det: 0.0f;
	18
	19 		float3 Rx = det*make_float3(M.z.z*M.y.y -
	20 		M.z.y*M.y.z, M.z.y*M.x.z - M.z.z*M.x.y, M.y.z*M.x.y - M.y.y*M.x.z);
	21
	22 		float3 Ry = det*make_float3(M.z.x*M.y.z -
	23 		M.z.z*M.y.x, M.z.z*M.x.x - M.z.x*M.x.z, M.y.x*M.x.z - M.y.z*M.x.x);
	24
	25 		float3 Rz = det*make_float3(M.z.y*M.y.x -
	26 		M.z.x*M.y.y, M.z.x*M.x.y - M.z.y*M.x.x, M.y.y*M.x.x - M.y.x*M.x.y);
	27
	28 		float3 T = -make_float3(M.x.w, M.y.w, M.z.w);
	29
	30 		R.x = make_float4(Rx.x, Rx.y, Rx.z, dot(Rx, T));
	31 		R.y = make_float4(Ry.x, Ry.y, Ry.z, dot(Ry, T));
	32 		R.z = make_float4(Rz.x, Rz.y, Rz.z, dot(Rz, T));
	33 		R.w = make_float4(0.0f, 0.0f, 0.0f, 1.0f);
	34
	35 		return R;
	36 	}


這段的原理, 可以從[這篇 blog](http://negativeprobability.blogspot.tw/2011/11/affine-transformations-and-their.html) 得到一些印證. 來解釋一下：

所謂 Affine space, 就是空間中的向量可以僅僅由線性組合就可轉換, 矩陣就以 affine matrix 表示.

圖學的 4x4 matrix 都是 3x3 兜上 vector __t__ 得到, i.e.


\begin{align}
\vec{x}^\prime = R\vec{x} + T
\end{align}

上式即為將 __x__ 轉換為 __x'__ 的變換
若 R 為 3x3 matrix, 則 4x4 的表示式為 __M__：


\begin{align}
\begin{array}{c|c}
R & T \\ \hline
0 &  1 \\
\end{array}
\end{align}

現在做一些移項：

\begin{align}
\vec{x}^\prime - T = R\vec{x}
\end{align}

\begin{align}
R^{-1}(\vec{x}^\prime - T) = \vec{x}
\end{align}

\begin{align}
\vec{x} = R^{-1}\vec{x}^\prime - R^{-1}T
\end{align}

對照（1）（2）的變換式, 則 $M^{-1}$ 為:

\begin{align}
\begin{array}{c|c}
R^{-1} & R^{-1}(-T) \\ \hline
0 &  1 \\
\end{array}
\end{align}


這個矩陣就是上面程式所要得到的結果, 現在來解說一下：

L7 ~ 17: 計算 R(3x3) 的 determinant, 如果 det(R) = 0 就把 element 為 0 的更換為最小浮點值, 因為我們要取 1/det(R).

L19 ~ 26: 先計算 $R^{-1}$, 根據[這裡](http://mathworld.wolfram.com/MatrixInverse.html)的反矩陣公式.

L28: $-T$ 即為(6)式右上角

L30 ~33: 即組合出 $M^{-1}$ (4x4)

希望以上 _非嚴謹證明_ 的對照法, 可以對解讀本段程式碼有幫助.
