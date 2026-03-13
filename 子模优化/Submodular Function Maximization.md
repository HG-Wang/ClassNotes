# Submodular funcitons

Submodularity is a property of set functions, i.e., functions $f : 2^V → R$  that assign each subset S ⊆ V a value$f (S)$. Hereby V is a ﬁnite set, commonly called the ground set.

我们假设 $f(\emptyset)=0$ , 也就是说空集不带有任何值

我们接下来了解一下子模性的两种等价定义，第一个定义依赖于 离散导数（discrete derivative） 的定义, 经常也叫做 marginal gain

## Definition 1.1

(Discrete derivative) For a set function $f: 2^V \rightarrow \mathbb{R}$, $S \subseteq V$, and $e \in V$, let $\Delta_{f}(e \mid S) := f(S \cup \{ e \}) - f(S)$be the discrete derivative of $f$at $S$with respect to $e$.

Where the function $f$is clear from the context, we drop the subscript and simply write $\Delta(e \mid S)$.