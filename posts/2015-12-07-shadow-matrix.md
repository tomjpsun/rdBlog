layout: post
title: "Shadow Matrix"
date: 2015-12-07 23:38:36 +0800
comments: true
has_math: True
Category: Math
Tags: Matrix
Has_Math: True

尋找如何做出 Shadow matrix 的過程中，發現 [HOTBALL'S HIVE](http://www.csie.ntu.edu.tw/~r89004/hive/shadow/page_1.html) 介紹得很好。但是有些推導過程，中間的步驟很簡明，對於新手的我來說，還是得拿紙筆來演練，才能順利接上，在此作個筆記。這裡的圖片引用自該網頁。
<!--More-->
目的：給定光源，在已知平面上 （以 normal vector **P** 表示）找出平面以外某一點 **V** ，在平面上的投影點，然後導出其 Matrix Operation。

![Project Diagram](http://www.csie.ntu.edu.tw/~r89004/hive/shadow/images/proj.jpg)

首先，從
\begin{align}\( \vec V \) + k\( \vec L \)
\end{align}


開始: 如圖，沿著射線，存在ㄧ k 值，使得

\begin{align}\( \vec V \) + k\( \vec L \) = \vec 0
\end{align}

為平面上的交點：

\begin{align}(\vec V + k \vec L) \cdot \vec P = 0
\end{align}
\begin{align} k = -\frac{\vec V \cdot \vec P}{\vec L \cdot \vec P}
\end{align}

所以投影點為：

\begin{align} \vec V + k \vec L &= \vec V - \frac{\vec V \cdot \vec P}{\vec L \cdot \vec P} \vec L \cr &= \frac{\vec V (\vec L \cdot \vec P) - (\vec V \cdot \vec P) \vec L}{\vec L \cdot \vec P} &(\ddagger) \end{align}

令

\begin{align} \vec L &= \langle L_x,L_y,L_z,0 \rangle \cr \vec V &= \langle V_x,V_y,V_z,1 \rangle \cr \vec P &= \langle a,b,c,d \rangle
\end{align}

對於 homogeneous coordinate
\begin{align}
 \langle x,y,z,w \rangle
\end{align}
而言，w 是分母之意， 亦即對應的 3D coordinate 為
\begin{align}  \langle \frac{x}{w}, \frac{y}{w}, \frac{z}{w} \rangle
， 也就是 \( \vec L \cdot \vec P \)
\end{align}


在 homogeneous coordinate 表示下恰為
\begin{align} \( w \) 之值。 展開 \((\ddagger)\) 式子的分子為：
\end{align}

\begin{align}
\langle V_x,V_y,V_z,1 \rangle (aL_x + bL_y + cL_z) - (aV_x + bV_y + cV_z + d) \langle L_x,L_y,L_z,0 \rangle
\end{align}

上式 x 分量為：

\begin{align}
V_x(bL_y + cL_z) - V_y(bL_x) - V_z(cL_x) - dL_x \newline
\end{align}

，同法可得 y,z 分量。

\begin{align}
整理得到M_{shadow} = \begin{bmatrix} bL_y + cL_z & -bL_x & -cL_x & -dL_x \cr -aL_y & aL_x + cL_z & -cL_y & -dL_y \cr -aL_z & -bL_z & aL_x + bL_y & -dL_z \cr 0 & 0 & 0 & aL_x + bL_y + cL_z \end{bmatrix}
\end{align}

所以空間中任意一點 V 的投影為：

\begin{align}
M_{shadow} \cdot \vec V
\end{align}


內心獨白：\begin\{align}LaTeX 打了會上癮！\end{align}
