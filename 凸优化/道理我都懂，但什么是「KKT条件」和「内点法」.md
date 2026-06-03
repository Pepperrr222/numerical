## 对偶优化问题

对于一个优化问题


$$
\begin{aligned}
\min_{x \in \mathbb{R}^n} \quad & f(x) \\
\text{s.t.} \quad & g_i(x) \leq 0, \quad i = 1, \dots, m \\
& h_j(x) = 0, \quad j = 1, \dots, p
\end{aligned}
$$

可行域为满足所有约束的点集：
$$
\mathcal{D} = \{ x \in \mathbb{R}^n \mid g_i(x) \leq 0, \, i=1,\dots,m; \; h_j(x) = 0, \, j=1,\dots,p \}
$$

我们想把它转化为无约束优化问题
第一步我们设想利用指示函数
$$F(x) = f(x) + I_{-}(g(x))$$
若$g(x) \leq 0$, $I_{-} = 0$
若$g(x) > 0$, $I_{-} = +\infty$
并将原问题转化为

$$\min_{x \in \mathbb{R}^n}  f(x)+ I_{-}(g(x))$$
这样的转化能使转化后的的问题与原问题有相同的解，但是目标函数的连续性消失了，且引入了无穷值

于是我们用线性函数代替

$$I_-(g(x))=\lambda g(x) $$
这样目标函数有了很好的性质，但是不同解。我们注意到可以对$\lambda$取最值, 得到等价问题






