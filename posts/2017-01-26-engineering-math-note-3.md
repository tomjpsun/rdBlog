title: "Engineering Math Note (3)"
date: 2017-01-26 17:21:22 +0800
has_math: True
Category: Math
Tags: Differential Equation
Has_Math: True


本篇是 Zill-Advanced Engineering Mathmatics 5th, Section 3.2 的筆記,

討論一個解題技術 _Reduction Of Order_, 可以把高階的微分方程式降階.

線性二階齊次微分方程式的解:


$$a_2(x)y'' + a_1(x)y' + a_0(x)y = 0 \tag 1$$

__要點是 利用代換法將兩次微分方程換成一次微分方程.稱為 Reduction Of Order__

在討論這個解之前，必須先要知道 __Theory of linear equations__:

線性 n 階齊次微分方程式為：

$$ a_{n}(x)y^{(n)} + a_{n-1}(x)y^{(n-1)} + ... + a_1(x)y' + a_0(x)y = 0 \tag 2$$


`Definition 3.1.3`

$$ y_1,y_2,...,y_n \, 為線性 \,n\, 階齊次微分方程式的 \,n\, 個線性獨立解, 稱為 \, fundamental\,set\,of\,solutions $$

`Theorem 3.1.4` Existance of a Fudamental Set:

$$ 線性 \,n\, 階齊次微分方程式必存在一組 \,fundamental\, set\, of\, solutions $$

`Theorem 3.1.5` General Solution - Homogeneous Equations:

$$ 令 y_1,y_2,...,y_n \, 是線性 \,n\, 階齊次微分方程式的 \,n\, 個線性獨立解, 則其 \,general\,solution\,為 $$
$$ y = c_1y_1(x) + c_2y_2(x) + ... + c_ny_n(x) \tag 3$$

根據上面的 theorems, (1) 式為二階, 故其 general solution 形式為：

$$ y = c_1y_1 + c_2y_2 $$

今天已知 $\,y_1\,$ 為一解, 根據“解必須線性獨立”的特性, 可以假設另一解$\, \cfrac{y}{y_1}=u(x) \,$

$$ y = y_1 u $$

$$ y' = uy_1' + u'y_1 \tag 4$$

$$ y'' = uy_1'' + 2y_1'u' + u''y_1 \tag 5$$

把(1)式簡化為:

$$ y'' + P(x)y' + Q(x)y = 0 \tag 6$$

把 (4) (5) 代入 (6):

$$ y'' + Py' + Qy = u[y_1''+Py_1' + Qy_1] + y_1u'' + (2y_1' + Py_1)u' = 0 $$

但是 $y_1$ 為一解, 意味著 $\,y_1''+Py_1'+Qy_1=0\,$, 故上式簡化為:

$$ y_1u'' + (2y_1' + Py_1)u' = 0 $$


let $w = u'$,(降階)


$$ y_1w' + (2y_1'+Py_1)w = 0 $$

$$ \cfrac{dw}{w} + 2\cfrac{y_1'}{y_1} + Pdx = 0$$

$$ ln \left|wy_1^2 \right| = - \int Pdx + c $$

or

$$ wy_1^2 =c_1e^{-\int Pdx} $$

since $w=u'$, we integrate again:

$$ u = c_1 \int \cfrac{e^{- \int Pdx}}{y_1^2} dx + c_2$$


令 $ \,c_1=1,\,c_2=0, and \,y = y(x)y_1(x),\,則\,(6)\,$式的解為：


$$ \boxed{y_2 = y_1(x)\int \cfrac{e^{- \int P(x)dx}}{y_1^2(x)}dx} \tag 7$$

上式可以很方便的讓我們從一個已知解得到第二個解.
