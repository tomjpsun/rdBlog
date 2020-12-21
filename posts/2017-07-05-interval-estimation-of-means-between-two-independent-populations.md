title: "Interval Estimation of Means Between Two Independent Populations
date: 2017-07-05 08:56:00 +0800
has_math: True
Category: Math
Tags: Statistics, Interval Estimation, Confidence Interval
Has_Math: True

$$ \newcommand{\sigmatwo}{\sqrt{ \frac{\sigma_1^2}{n_1} + \frac{\sigma_2^2}{n_2}}} $$
$$ \newcommand{\stwo}{\sqrt{ \frac{s_1^2}{n_1} + \frac{s_2^2}{n_2}}} $$

A.$(1-\alpha) 100 \%$ Confidence Interval for $(\mu_1-\mu_2)$

case I: $n_1$ > 30, $n_2$ > 30

Since

$$Var(y_1-y_2) = Var(y_1) + Var(y_2)$$

$$\sigma_{y_1-y_2} = \sigmatwo$$

therefore：

$$(\bar y_1 - \bar y_2) \pm Z \sigmatwo \\
\simeq (\bar y_1 - \bar y_2) \pm Z \stwo \tag{1}$$

____

case II: $min(n_1, n_2) < 30$ and both $\sigma_1, \sigma_2$ are unknown:

把 (1)的 Z 換成 t 分佈來估計

$$ (\bar X_1 - \bar X_2) \pm t \stwo \tag{2}$$


case III: $\sigma_1 = \sigma_2$ in case II:

令 $\sigma_1^2 = \sigma_2^2 = \sigma^2$

此時兩組 samples 可以合併計算 標準差, 令其為 $S_p$ (p 表示 pooled), 則:

由取樣之變異數公式：

$$s^2 = \Sigma \frac{(x-\bar x)^2}{n-1}$$

合併兩群取樣後的變異數：上面放取樣變異總和, 下面放總樣本數-2

$$S_p^2 = \frac{\sum_{i=1}^{n_1} (X_i - \bar X_1)^2 + \sum_{j=1}^{n_2} (X_j - \bar X_2)^2}{n_1 + n_2 -2} \tag{3.1}$$

換成 $s_1, s_2$ 表示:

$$ S_p^2 = \frac{ (n_1 - 1)s_1^2 + (n_2 -1)s_2^2 }{n_1 + n_2 - 2} \tag{3.2}$$

又, $s_p^2 \simeq \sigma_1^2 = \sigma_2^2$ 代入 (2) 式：

$$ (\bar X_1 - \bar X_2) \pm t \sqrt{s_p^2 (\frac{1}{n_1}+\frac{1}{n_2})} \tag{3}$$
