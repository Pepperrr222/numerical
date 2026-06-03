# Lagrange对偶函数

Lagrange对偶是[[对偶理论]]的核心构造：通过将约束"价格化"，将原约束优化问题松弛为无约束优化问题，获得最优值的下界。

[[对偶理论]] | [[凸优化问题标准形式]] | [[KKT条件]]

---

## 一、Lagrange函数

### 1.1 构造

考虑优化问题（不一定凸）：

$$\begin{aligned} \min \quad & f_0(\mathbf{x}) \\ \text{s.t.} \quad & f_i(\mathbf{x}) \leq 0, \quad i=1,\ldots,m \\ & h_i(\mathbf{x}) = 0, \quad i=1,\ldots,p \end{aligned}$$

**Lagrange函数** $L: \mathbb{R}^n \times \mathbb{R}^m \times \mathbb{R}^p \to \mathbb{R}$：

$$L(\mathbf{x}, \boldsymbol{\lambda}, \boldsymbol{\nu}) = f_0(\mathbf{x}) + \sum_{i=1}^{m} \lambda_i f_i(\mathbf{x}) + \sum_{i=1}^{p} \nu_i h_i(\mathbf{x})$$

- $\boldsymbol{\lambda} = (\lambda_1, \ldots, \lambda_m)$：**Lagrange乘子**（关联不等式约束）
- $\boldsymbol{\nu} = (\nu_1, \ldots, \nu_p)$：**Lagrange乘子**（关联等式约束）
- $\text{dom }L = \mathcal{D} \times \mathbb{R}^m \times \mathbb{R}^p$，其中 $\mathcal{D} = \bigcap_{i=0}^{m} \text{dom }f_i \cap \bigcap_{i=1}^{p} \text{dom }h_i$

### 1.2 解释

Lagrange函数 = 目标函数 + 约束的**加权和**，权重 $\lambda_i$ 可理解为违反约束 $f_i \leq 0$ 的"罚价格"。

---

## 二、Lagrange对偶函数

### 2.1 定义

$$\boxed{g(\boldsymbol{\lambda}, \boldsymbol{\nu}) = \inf_{\mathbf{x} \in \mathcal{D}} L(\mathbf{x}, \boldsymbol{\lambda}, \boldsymbol{\nu})}$$

$$= \inf_{\mathbf{x} \in \mathcal{D}} \left(f_0(\mathbf{x}) + \sum_{i=1}^{m} \lambda_i f_i(\mathbf{x}) + \sum_{i=1}^{p} \nu_i h_i(\mathbf{x})\right)$$

### 2.2 基本性质

1. **$g$ 恒为凹函数**（无论原问题是否凸）
   - $L(\mathbf{x}, \boldsymbol{\lambda}, \boldsymbol{\nu})$ 对 $(\boldsymbol{\lambda}, \boldsymbol{\nu})$ 是仿射的
   - 凹函数 = 一族仿射函数的逐点下确界

2. **下界性质**：对任意 $\boldsymbol{\lambda} \succeq \mathbf{0}$ 和任意 $\boldsymbol{\nu}$：
   $$g(\boldsymbol{\lambda}, \boldsymbol{\nu}) \leq p^*$$
   即：**任何可行的对偶变量给出最优值 $p^*$ 的下界**

### 2.3 对偶函数的解析计算

对许多标准问题，$g(\boldsymbol{\lambda}, \boldsymbol{\nu})$ 可以通过最小化 $\mathbf{x}$ 解析求得。

**例1：LP的对偶函数**

原LP：$\min \mathbf{c}^T\mathbf{x}$ s.t. $\mathbf{A}\mathbf{x} = \mathbf{b}, \mathbf{x} \succeq 0$

$L(\mathbf{x}, \boldsymbol{\nu}) = \mathbf{c}^T\mathbf{x} + \boldsymbol{\nu}^T(\mathbf{b} - \mathbf{A}\mathbf{x}) = (\mathbf{c} - \mathbf{A}^T\boldsymbol{\nu})^T\mathbf{x} + \boldsymbol{\nu}^T\mathbf{b}$

$g(\boldsymbol{\nu}) = \inf_{\mathbf{x} \succeq 0} (\mathbf{c} - \mathbf{A}^T\boldsymbol{\nu})^T\mathbf{x} + \boldsymbol{\nu}^T\mathbf{b}$

- 若 $\mathbf{c} - \mathbf{A}^T\boldsymbol{\nu} \succeq 0$：$g(\boldsymbol{\nu}) = \boldsymbol{\nu}^T\mathbf{b}$
- 否则：$g(\boldsymbol{\nu}) = -\infty$

**例2：QP的对偶函数**

原QP（$\mathbf{P} \succ 0$）：$\min \frac{1}{2}\mathbf{x}^T\mathbf{P}\mathbf{x} + \mathbf{q}^T\mathbf{x}$ s.t. $\mathbf{A}\mathbf{x} = \mathbf{b}$

$L(\mathbf{x}, \boldsymbol{\nu}) = \frac{1}{2}\mathbf{x}^T\mathbf{P}\mathbf{x} + \mathbf{q}^T\mathbf{x} + \boldsymbol{\nu}^T(\mathbf{A}\mathbf{x} - \mathbf{b})$

对 $\mathbf{x}$ 求最小：$\nabla_\mathbf{x} L = \mathbf{P}\mathbf{x} + \mathbf{q} + \mathbf{A}^T\boldsymbol{\nu} = \mathbf{0}$ ⇒ $\mathbf{x} = -\mathbf{P}^{-1}(\mathbf{q} + \mathbf{A}^T\boldsymbol{\nu})$

代入：$g(\boldsymbol{\nu}) = -\frac{1}{2}\boldsymbol{\nu}^T\mathbf{A}\mathbf{P}^{-1}\mathbf{A}^T\boldsymbol{\nu} - (\mathbf{b} + \mathbf{A}\mathbf{P}^{-1}\mathbf{q})^T\boldsymbol{\nu} - \frac{1}{2}\mathbf{q}^T\mathbf{P}^{-1}\mathbf{q}$

---

## 三、对偶可行域

对偶变量 $(\boldsymbol{\lambda}, \boldsymbol{\nu})$ 称为**对偶可行**，若：

$$\boldsymbol{\lambda} \succeq \mathbf{0} \quad \text{且} \quad g(\boldsymbol{\lambda}, \boldsymbol{\nu}) > -\infty$$

$\text{dom }g = \{(\boldsymbol{\lambda}, \boldsymbol{\nu}) \mid \boldsymbol{\lambda} \succeq \mathbf{0}, g(\boldsymbol{\lambda}, \boldsymbol{\nu}) > -\infty\}$ 是**凸集**（凹函数有效定义域的性质）。

---

## 四、对偶函数与共轭函数

Lagrange对偶函数可以用[[共轭函数|Fenchel共轭]]表达。

对一般优化问题 $\min_{\mathbf{x}} f_0(\mathbf{x})$ s.t. $\mathbf{A}\mathbf{x} = \mathbf{b}$：

$$g(\boldsymbol{\nu}) = -\mathbf{b}^T\boldsymbol{\nu} - f_0^*(-\mathbf{A}^T\boldsymbol{\nu})$$

其中 $f_0^*$ 是 $f_0$ 的共轭。

建立了 **Lagrange对偶 ⇔ Fenchel对偶** 的等价性。

---

## 五、几何解释

### 5.1 达到下界

令 $\mathcal{G} = \{(f_1(\mathbf{x}),\ldots,f_m(\mathbf{x}),h_1(\mathbf{x}),\ldots,h_p(\mathbf{x}),f_0(\mathbf{x})) \mid \mathbf{x} \in \mathcal{D}\}$（可达约束-目标值集合）。

对偶函数 $g(\boldsymbol{\lambda}, \boldsymbol{\nu})$ 给出 $\mathcal{G}$ 与一个超平面的最小截距：

$$g(\boldsymbol{\lambda}, \boldsymbol{\nu}) = \inf\{(\boldsymbol{\lambda}, \boldsymbol{\nu}, 1)^T \cdot (\mathbf{u}, \mathbf{v}, t) \mid (\mathbf{u}, \mathbf{v}, t) \in \mathcal{G}\}$$

即 Lagrange 乘子定义了一个超平面族，$g$ 是这个超平面族给出的最大下界。

### 5.2 对偶间隙的几何来源

$\mathcal{G}$ 的**凸包**与 $p^*$ 之间的"间隙"恰恰导致了对偶间隙 $p^* - d^*$。若 $\mathcal{G}$ 在某个意义下是凸的（如凸问题满足Slater条件），则间隙为0。

---

## 相关笔记

- [[对偶理论]] — Lagrange对偶总览
- [[弱对偶性与强对偶性]] — 对偶间隙与零间隙条件
- [[Slater条件与约束品性]] — 强对偶的充分条件
- [[共轭函数]] — Fenchel对偶与Lagrange对偶的等价
- [[KKT条件]] — 对偶最优解充要条件
