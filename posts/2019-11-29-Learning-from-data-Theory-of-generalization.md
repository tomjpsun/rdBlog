Title: "Learning from data : Theory of generalization
date: 2019-11-29 22:30:00 +0800
Category: Machine Learning
Tags: Shatter, Breakpoint, Growth Function, Vc Dimension
Has_Math: True

在林軒田老師的第六週課程, 要逐漸推導 __機器學習是否可能__? 搭配 [textbook Section](https://www.tenlong.com.tw/products/9781600490064) 2.1 研讀,
對於幾個專有名詞還是想不透徹, 發現這裡解釋的很清楚, 特別紀錄一下.

[註]本篇圖片來自老師的 textbook, 與上課的投影片.

#如何理解 shatter, breakpoint, 與 VC dimension?

這篇有很好的分析, 因為是課程的補充說明, 建議先過完軒田老師解釋後再閱讀:
[https://www.zhihu.com/question/38607822/answer/151561258](https://www.zhihu.com/question/38607822/answer/151561258)


#名詞定義：

_Dichotomy_:

Let $x_1,\dots,x_N \in \chi$ (sample data), under the Hypothesis $\mathcal{H}$, the
_dichotomies_ is defined as:

$$\mathcal{H}(x_1,\dots,x_N) =  \big\\{ \big( h(x_1),\dots,h(x_N) \big)  | h \in \mathcal{h} \big\\}   \tag{1}$$

其中 h 是一個對應到二元素的函數, 例如直線的左右邊. 舉例來說, 如果 $h:x \mapsto \{+1, -1\}$, 則 _dichotomies_ of $\mathcal{H}$, 就是所有的二元組合: $(1, 1, \dots, 1), (1, 1, \dots, -1),\dots$


_成長函數_:

由 Hypothesis $\mathcal{H}$ 能夠產生出來的 _dichotomies_ 最多的個數,. 記做:

$$ m_H(N) = \max_{x_1,\dots,x_N \in \chi} \vert \mathcal{H}(x_1,\dots,x_N) \vert \tag{2}$$

其中 $\vert . \vert$ 是取 cardinal number 的意思.


由以上定義知道, $m_H(N)$ 最多是 $\{+1,-1\}^N$ 的個數, 亦即 $m_H(N) \leq 2^N$

為什麼會有不等號呢？接下來圖例說明比較清楚：在2D平面上，將 N 個點分類, 可以找到幾種方法？

這個問題牽涉到點分佈的幾何關係, 還有分類的方式,

![](/images/dichotomy_3_points.png)

如圖(a), N=3, 三個點無法被線性劃分, 圖(b)呢？

![](/images/dichotomy_3_points_detail.png)

完全可以被線性劃分, 有 8 條可能的 lines.

![](/images/dichotomy_4_points_detail.png)

本圖 N=4, 有 2 種情形無法被線性劃分, 其他 14 種可以 -> 有 14 條可能的 lines.


說了這麼多, 就是要說明 $m_H(N) \leq 2^N$ 不等式是可能存在的.

我們希望在 Hoffding inequality 裡面降低 $M$ 這個數量，讓機器學習這件事變得有機會：

$$ P\big[\vert E_{in}(g)-E_{out}(g)\vert \gt \epsilon\big] \le 2 M \exp(-2\epsilon^2N) \tag{3}$$

其中 $E_{in}$ 是 Hypothesis 對於 sample data 的期望值.

$E_{out}$ 是 Hypothesis 對於未來 data 的期望值.

$M$ 是所有 Hypothesis 的數量.(詳見第四週內容)

我們希望他們夠接近，就表示我們選到的這個 Hypothesis function 將來表現會夠好，代表 _機器學習_ 是可行的,

也就是我們想找到一個合理的 $M$ 讓左邊機率夠小, 即右邊 bound 住左邊, 而不是無限大.

截至目前為止, $M$ 可以成長函數 $m_H(N)$ 取代, 其 upper bound 是 $2^N$.

觀察一下, $M_H(2)=4, M_H(3)=8, M_H(4)=14$ , 偏離 $2^N$ 了, 為什麼？我們來研究一下.


我們説, 從 N=k 開始, $m_H(N) \lt 2^k$, 那麼 N=k+1 時, $M_H(k)$ 會不會繼續小於 $2^{k+1}$ 呢？

會的!

![](/images/break_points_on_5.png)

從原本 N 點的情形, 可以引導出 N+1 點的情形, 我們稱那個 min breakpoint 為 成長函數 $m_H(N)$ 的 break point.

我們説：從 break point 開始, h 開始不能 shatter 資料, 換句話說  $m_H(N) \lt 2^k$, 由此例子引出什麼是 shatter:

_Shatter_:

通俗的説, 就是所有的 dichotomy, 都可以被 hypothesis function $h$ __區分出來__ (或說 __打散__ ).
所以，unshatterable 就是指在 break point 出現的時候, 開始有某些 dichotomies 是 h 無法 shatter,
這是個重要的指標, 意味著 $m_H(N)$ 不再追隨 $2^N$ 的成長,
教科書 Definition 2.4 給出了 B(N,k) 為其 upper bound, 是以 polynomial 成長. 證明以後有機會再補上,
這裡就省略了.

我們只要知道, (3) 式的 $M$ 已經可以降到 polynomial 等級了, 我們接近 "學習是可行的" 這件事又進了一步.

_VC Dimension_:

The Vapnik- Chervonenkis dimension of a hypothesis set $\mathcal{H}$,
denoted by $d_{vc} (\mathcal{H})$ or simply $d_{vc}$, is the largest value of $N$ for which $m_H(N)$ =
$2^N$ . If $m_H(N) = 2^N$ for all $N$, then $d_{vc} (\mathcal{H}) = \infty$.

這個定義其實是説, 到 break point 出現之前, Hypothesis set 選出來的 h 都可以 shatter data(遵循 $2^N$)
的那個最大的 $N$, 稱為 VC Dimension. 說穿了, 它其實就是我們的 break point k 再減 1 的值.

$$ d_{vc} = k - 1 \tag{4} $$
