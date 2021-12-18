title: "why-conditional-independent-can-reduce-complexity"
date: 2019-05-29 15:31:00 +0800
Category: Math
Tags: Statistics, Native Bayes
Has_Math: True

令 $x_1,x_2,...,x_n, y$ 都是 binary variable, 即 0 或 1.

要計算

$$P(x_1,x_2,...,x_n | y) \tag 1 $$

需要算幾個機率呢？

y = 0 的情形, 我們要找到

$P(x_1 = 0, x_2 = 0,..., x_n = 0 | y = 0) + P(x_1 = 0, x_2 = 0,..., x_n = 1 | y = 0) + \cdots \\
P(x_1 = 1, x_2 = 1,..., x_n = 1 | y = 0)$

共需要找出 $2^n$ 個機率才可算 (1) 式,

complexity is $\Theta(2^n)$

y = 1 的次數也如此, 兩者共計 $\Theta(2 \cdot 2^n)$ 次

那麼如果 $x_1, x_2, \cdots , x_n$ 是 conditionally independent variables,

則

$$P(x_1,x_2,...,x_n | y) = P(x_1 | y) \cdot P(x_2 | y) \cdots P(x_n | y) \tag 2$$

其中

$P(x_1 | y)$ 需要找出 $P(x_1 = 0 | y = 0), P(x_1 = 0 | y = 1)$ 兩個機率


$P(x_2 | y)$ 需要找出 $P(x_2 = 0 | y = 0), P(x_2 = 0 | y = 1)$ 兩個機率


$\cdots$


$P(x_n | y)$ 需要找出 $P(x_n = 0 | y = 0), P(x_n = 0 | y = 1)$ 兩個機率

共需要找出 $2n$ 個機率即可算 (2) 式

complexity is $\Theta(2n)$

	這裡有個可能混淆的地方, 雖然 (2) 式右邊是連乘積, 但是準備動作是 counting 我們需要那些機率,

	也就是說：complexity 是指準備這些機率的 cost, 並非拿去相乘.


故在 native bayes 假設的條件( 即 independent conditional variables) 的情況下,

(1) 式可以拆成 (2) 式來算, 大幅度減少了 computation complexity.
