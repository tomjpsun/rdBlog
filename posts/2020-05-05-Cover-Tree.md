Title: "Cover Tree"
date: 2020-05-05 10:01:00 +0800
Category: Algorithm
Tags: Tree
Has_Math: True

__The Cover Tree__

Cover Tree 是 2006 年由 J.Langford 等人提出的 Algorithm, 適合用在找 nearest neighbors, 他的介紹網站在 [https://hunch.net/~jl/projects/cover_tree/cover_tree.html](https://hunch.net/~jl/projects/cover_tree/cover_tree.html)

以下節錄一些定義： [來源](https://smartech.gatech.edu/handle/1853/54354)

A cover tree $\mathscr{T}$ on a dataset $S$ is a leveled tree where each level is a “cover”
for the level beneath it. Each level is indexed by an integer scale $s_i$ which
decreases as the tree is descended. Every node in the tree is associated with a
point in $S$ . Each point in $S$ may be associated with multiple nodes in the tree;
however, we require that any point appears at most once in every level. Let $C_{s_i}$
denote the set of points in S associated with the nodes at level $s_i$. The cover
tree obeys the following invariants for all $s_i$:

$(Nesting). C_{s_i} \subset C_{s_i - 1}.$ This implies that once a point $p \in S$ appears in
$C_{s_i}$ then every lower level in the tree has a node associated with $p$.

$(Covering tree)$. For every $p_i \in C_{s_i - 1}$, there exists a $p_j \in C_{s_i}$ such that
$d(p_i,p_j) < 2^{s_i}$ and the node in level $s_i$ associated with $p_j$ is a parent of the
node in level $s_i - 1$ associated with $p_i$.

$(Separation)$. For all distinct $p_i, p_j \in C_{s_i}, d(p_i, p_j) > 2^{s_i}$ .

很模糊是吧？ 這個清楚的[例子](http://users.cecs.anu.edu.au/~qshi/talk/introduction%20to%20covertree060815.pdf)看完就懂了！

__The Expension Constant__

Let $B_S (p, \Delta)$ be the set of points in $S$ within a closed ball of radius $\Delta$ around
some $p \in S$ with respect to a metric $d: B_S (p, \Delta) = \{ r \in S : d(p; r)  \leq \Delta \}$ . Then, the
expansion constant of S with respect to the metric d is the smallest $c \geq 2$ such that
$$\vert B_S (p, 2\Delta) \vert \leq c \vert B_S (p, \Delta) \vert \quad \forall p \in S, \forall \Delta \gt 0 \tag 1 $$

我們改用口語描述 Expension Constant： 就是`一個節點，下一階的節點數量的最小值`

(1)左式  $\vert B_S (p, 2\Delta) \vert$ 代表上一階的節點範圍 $2\Delta$ 內的點數量

(1)右式  $\vert B_S (p, \Delta) \vert$ 代表本階的節點範圍 $\Delta$ 內的點數量，
取本階 nodes 代表的點時，有許多選擇，但是會出現某種選法，使得本階從上階分裂出來的節點儘量少。
這個最小量就是 c.
