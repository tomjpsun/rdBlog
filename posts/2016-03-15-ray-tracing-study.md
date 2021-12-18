layout: post
title: "Ray Tracing Study"
date: 2016-03-15 19:48:40 +0800
comments: true
Category: Blender
Tags: Study, Ray Tracing
Has_Math: True

這幾天開始研究 ran tracing, [Scratchapixel 2.0](http://www.scratchapixel.com/lessons/3d-basic-rendering/introduction-to-ray-tracing) 的入門介紹得還不錯！推薦！
<!--More-->
花最多時間的是卡在 Source code 裡 Snell Law 的套用, 因為這部分屬於折射相關的計算, [跑去 ptt 求救](https://www.ptt.cc/bbs/Physics/M.1457925341.A.63F.html)才得到答案, ptt 實在是臥虎藏龍啊！

這裡的關鍵就是[這篇論文](http://pjreddie.com/media/files/Redmon_Thesis.pdf)的式子 (3.14):

\begin{align} {\bf T} = \alpha {\bf I} + \beta {\bf N}
\end{align}

直接由入射線 I 與交點法向量 N 得到 折射線 T, 太神了！

Sample code 裡面的部分,

	177   float k = 1 - eta * eta * (1 - cosi * cosi);
	178   Vec3f refrdir = raydir * eta + nhit * (eta * cosi - sqrt(k));

就是直接套用 (3.14) 的解 (3.26):

\begin{align} {\bf R} = \frac{\eta_i}{\eta_j}{\bf I} - \bigg[ \frac{\eta_i}{\eta_j} \big( {\bf I} \cdot {\bf N} \big) + \sqrt{ 1 + \big( \frac{\eta_i}{\eta_j}  \big) ^2 \big[ \big( {\bf I} \cdot {\bf N} \big) ^2 - 1 \big]} \bigg] {\bf N}
\end{align}



