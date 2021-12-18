layout: post
title: "Normal Vector Transformation"
date: 2016-01-18 18:21:05 +0800
comments: true
has_math: True
Category: Math
Tags: Matrix
Has_Math: True



當我們 transform 每一個 vertex 的時候，如何也 transform 它的 normal vector 呢？

這裡有詳細的說明：

[http://stackoverflow.com/questions/13654401/what-is-the-logic-behind-transforming-normals-with-the-transpose-of-the-inverse](http://stackoverflow.com/questions/13654401/what-is-the-logic-behind-transforming-normals-with-the-transpose-of-the-inverse)

<!--More-->

其中最簡單明白的演繹如下：

令 __N__ 是該點的法向量, __V__ 是該點的切向量

\begin{align} N \cdot V = 0
\end{align}

把向量內積用矩陣乘法表示：


\begin{align} N^{t} V = 0
\end{align}


\begin{align} N^{t} (M^{-1} M) V = 0
\end{align}


\begin{align} (N^{t} M^{-1})(M V) = 0
\end{align}

轉換回內積表示：

\begin{align} (N^{t} M^{-1})^{t}\cdot(M V) = 0
\end{align}

其中 __M____V__ 是 transform 後的切向量, 所以前項是 transform 後的法向量。

使用線性代數的定理：__A__, __B__ 為矩陣

\begin{align} (AB)^{t} = B^{t}A^{t}
\end{align}

上式的前項可以寫成:

\begin{align} (N^{t} M^{-1})^{t} = M^{-t}N
\end{align}


即是對原來的 transform matrix __M__ 取 transpose inverse 就可以 apply to normal vector __N__ 的意思。

這解釋了[在某些 Shader Code](http://www.tomdalling.com/blog/modern-opengl/06-diffuse-point-lighting/)裡面為何會有 transpose inverse matrix 的動作。

#
