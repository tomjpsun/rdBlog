layout: post
title: "Perspective Matrix"
date: 2016-01-04 10:11:26 +0800
comments: true
has_math: True
Category: Math
Tags: Matrix
Has_Math: True

本篇要來探討 perspective matrix 是如何形成的。
參考文件 :

[http://www.songho.ca/opengl/gl_projectionmatrix.html](http://www.songho.ca/opengl/gl_projectionmatrix.html)

<!--More-->

名詞解釋：

__NDC__: Normalized Device Coordinates － 所有維度都是在 [-1...1] 的範圍內的座標系統, __z__ 軸方向相反

__Perspective Frustum__: 四角錐體（金字塔）截取 far and near 兩個面.
x 範圍是 [-r...r], y 範圍是 [-t...t], z 範圍是 [－n...－f]

![](/images/perspective matrix frustum.png" alt="Drawing" style="width: 600px;"/>

我們要處理的點 :
\begin{align} \begin{bmatrix}
x_e & y_e & z_e & w_e
\end{bmatrix}\end{align}

我們要處理的點在 near plane 的投影點:
\begin{align} \begin{bmatrix}
x_p & y_p & z_p & w_p
\end{bmatrix}\end{align}

我們要處理的點，對應到 NDC space 的點（圖示沒有畫出來）:
\begin{align} \begin{bmatrix}
x_n & y_n & z_n
\end{bmatrix}\end{align}

從橫軸看:

![](/images/perspective matrix frustum side view.png)


依據上圖得知：

\begin{align}
x_p = n\cdot\frac{x_e}{-z_e}
\cr
\cr
y_p = n\cdot\frac{y_e}{-z_e}
\end{align}

對於 frustum 內的任何一點 e 都有：

\begin{align}
\begin{bmatrix} x_p \cr y_p \cr z_p \cr w_p \end{bmatrix} ＝ M_{projection} \cdot \begin{bmatrix} x_e \cr y_e \cr z_e \cr w_e \end{bmatrix}
\end{align}
其中 __We__ 是擴展出來的數, 應當為 1

我們就是要找出這個式子的 projection matrix:


轉換到 NDC 即：
\begin{align}
\begin{bmatrix} x_n \cr y_n \cr z_n \end{bmatrix} = \begin{bmatrix} \frac {x_p}{w_p} \cr \frac {y_p}{w_p} \cr \frac {z_p}{w_p} \end{bmatrix}
\end{align}

注意： 支援 OpenGL 的硬體會幫我們做這個除法（i.e. 齊次座標 轉 NDC）

因此，我們要湊出令 \begin{align} w_c = -z_e \end{align} 的投影結果，projection matrix 的第四列必須為：

\begin{align}
\begin{bmatrix} x_p \cr y_p \cr z_p \cr w_p \end{bmatrix} ＝ \begin{bmatrix} \cdot &\cdot & \cdot & \cdot \cr \cdot &\cdot & \cdot & \cdot \cr \cdot &\cdot & \cdot & \cdot \cr 0 & 0 & -1 & 0 \cr \end{bmatrix} \cdot \begin{bmatrix} x_e \cr y_e \cr z_e \cr w_e \end{bmatrix}
\end{align}

現在直接利用 Normalize, 找出與 point __e__ 的關係：

\begin{align}
x_n = \frac{x_p}{r} (代入 p) = \frac{n \cdot x_e}{-z_e \cdot r} = \frac{\frac{n}{r}\cdot x_e}{-z_e}
\end{align}

同理:

\begin{align}
y_n = \frac{y_p}{t} (代入 p) = \frac{n \cdot y_e}{-z_e \cdot t} = \frac{\frac{n}{t}\cdot y_e}{-z_e}
\end{align}


比較係數, projection matrix 的第一、二列為：

\begin{align}
\begin{bmatrix} x_p \cr y_p \cr z_p \cr w_p \end{bmatrix} ＝ \begin{bmatrix} \frac{n}{r} &0 & 0 & 0 \cr 0 &\frac{n}{t} & 0 & 0 \cr \cdot &\cdot & \cdot & \cdot \cr 0 & 0 & -1 & 0 \cr \end{bmatrix} \cdot \begin{bmatrix} x_e \cr y_e \cr z_e \cr w_e \end{bmatrix}
\end{align}

__z__ 與 __x__,__y__ 無關，所以上式可以寫成:

\begin{align}
\begin{bmatrix} x_p \cr y_p \cr z_p \cr w_p \end{bmatrix} ＝ \begin{bmatrix} \frac{n}{r} &0 & 0 & 0 \cr 0 &\frac{n}{t} & 0 & 0 \cr 0 & 0 & A & B \cr 0 & 0 & -1 & 0 \cr \end{bmatrix} \cdot \begin{bmatrix} x_e \cr y_e \cr z_e \cr w_e \end{bmatrix}
\end{align}


i.e.

\begin{align}
z_n = \frac{z_n}{w_n} = \frac{A \cdot z_e + B \cdot w_e}{-z_e} (w_e = 1) = \frac{A \cdot z_e + B}{-z_e}
\end{align}



利用 __Ze__ 範圍 [-n ... -f] normalize 之後, 對應到 __Zn__ 範圍 [-1 ... 1] 這個條件, 可求得 A, B :

\begin{align}
\frac{A \cdot (-n) + B}{n} = -1
\cr
\frac{A \cdot (-f) + B}{f} = 1
\end{align}


\begin{align}
A \cdot (-n) + B = -n
\cr
A \cdot (-f) + B = f
\end{align}

兩式相減得：

\begin{align}
A ＝ \frac{n+f}{n-f}
\end{align}

A 代回去求 B:

\begin{align}
B ＝ (1+A)\cdot f = \frac{n+f+n-f}{n-f} \cdot f = \frac{2nf}{n-f}
\end{align}

我們的 perspective matrix 為:

\begin{align}
\begin{bmatrix} x_p \cr y_p \cr z_p \cr w_p \end{bmatrix} ＝ \begin{bmatrix} \frac{n}{r} &0 & 0 & 0 \cr 0 &\frac{n}{t} & 0 & 0 \cr 0 & 0 & \frac{n+f}{n-f} & \frac{2nf}{n-f} \cr 0 & 0 & -1 & 0 \cr \end{bmatrix} \cdot \begin{bmatrix} x_e \cr y_e \cr z_e \cr w_e \end{bmatrix}
\end{align}
