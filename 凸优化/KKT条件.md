# KKT条件 (Karush-Kuhn-Tucker Conditions)

[[KKT条件]]是[[凸优化]]中最核心的最优性条件。在可微、强对偶成立的凸优化中，KKT条件是**充要条件**。

[[对偶理论]] | [[Slater条件与约束品性]] | [[弱对偶性与强对偶性]] | [[Lagrange对偶函数]]

---

## 一、KKT条件的四个组成部分

考虑问题：

$$\begin{aligned} \min \quad & f_0(\mathbf{x}) \\ \text{s.t.} \quad & f_i(\mathbf{x}) \leq 0, \quad i=1,\ldots,m \\ & h_i(\mathbf{x}) = 0, \quad i=1,\ldots,p \end{aligned}$$

假设 $f_i, h_i$ 可微（不一定凸）。

### 条件1：原始可行性 (Primal Feasibility)

$$f_i(\tilde{\mathbf{x}}) \leq 0, \quad i=1,\ldots,m$$
$$h_i(\tilde{\mathbf{x}}) = 0, \quad i=1,\ldots,p$$

### 条件2：对偶可行性 (Dual Feasibility)

$$\tilde{\lambda}_i \geq 0, \quad i=1,\ldots,m$$

### 条件3：互补松弛性 (Complementary Slackness)

$$\boxed{\tilde{\lambda}_i f_i(\tilde{\mathbf{x}}) = 0, \quad i=1,\ldots,m}$$

**解释**：
- 若约束是松弛的（$f_i(\tilde{\mathbf{x}}) < 0$），则乘子必为零 $\tilde{\lambda}_i = 0$
- 若乘子为正（$\tilde{\lambda}_i > 0$），则约束必为紧的（$f_i(\tilde{\mathbf{x}}) = 0$）
- 只有紧约束影响最优解

### 条件4：驻点条件 (Stationarity)

$$\boxed{\nabla f_0(\tilde{\mathbf{x}}) + \sum_{i=1}^{m} \tilde{\lambda}_i \nabla f_i(\tilde{\mathbf{x}}) + \sum_{i=1}^{p} \tilde{\nu}_i \nabla h_i(\tilde{\mathbf{x}}) = \mathbf{0}}$$

**解释**：在最优解处，目标函数的梯度可表示为约束梯度的**非负线性组合**（不等式约束）+ 任意线性组合（等式约束）。

**物理意义**：在最优解处，没有可以减小目标且保持可行的方向。

---

## 二、KKT条件的矩阵形式

$$\boxed{\begin{cases} \mathbf{f}(\tilde{\mathbf{x}}) \preceq \mathbf{0}, \quad \mathbf{h}(\tilde{\mathbf{x}}) = \mathbf{0} \quad & \text{(原始可行)} \\ \tilde{\boldsymbol{\lambda}} \succeq \mathbf{0} \quad & \text{(对偶可行)} \\ \tilde{\lambda}_i f_i(\tilde{\mathbf{x}}) = 0 \quad & \text{(互补松弛)} \\ \nabla f_0(\tilde{\mathbf{x}}) + D\mathbf{f}(\tilde{\mathbf{x}})^T \tilde{\boldsymbol{\lambda}} + D\mathbf{h}(\tilde{\mathbf{x}})^T \tilde{\boldsymbol{\nu}} = \mathbf{0} \quad & \text{(驻点)} \end{cases}}$$

其中 $D\mathbf{f}$ 和 $D\mathbf{h}$ 为Jacobian矩阵。

---

## 三、凸优化中的KKT定理

### 3.1 充要条件定理

**定理**：设 $f_i$ 为可微凸函数，$h_i$ 为仿射函数，且**强对偶成立**（如Slater条件满足）。

则 $\tilde{\mathbf{x}}$ 和 $(\tilde{\boldsymbol{\lambda}}, \tilde{\boldsymbol{\nu}})$ 分别为原始和对偶最优解 **当且仅当** KKT条件成立。

### 3.2 充分性证明（KKT ⇒ 最优）

若 $(\tilde{\mathbf{x}}, \tilde{\boldsymbol{\lambda}}, \tilde{\boldsymbol{\nu}})$ 满足KKT：

由驻点条件知 $\tilde{\mathbf{x}}$ 最小化 $L(\mathbf{x}, \tilde{\boldsymbol{\lambda}}, \tilde{\boldsymbol{\nu}})$（因为 $L$ 关于 $\mathbf{x}$ 是凸的，驻点 ⇔ 全局最小点）。

因此：

$$g(\tilde{\boldsymbol{\lambda}}, \tilde{\boldsymbol{\nu}}) = L(\tilde{\mathbf{x}}, \tilde{\boldsymbol{\lambda}}, \tilde{\boldsymbol{\nu}}) = f_0(\tilde{\mathbf{x}}) + \sum \tilde{\lambda}_i f_i(\tilde{\mathbf{x}}) + \sum \tilde{\nu}_i h_i(\tilde{\mathbf{x}}) = f_0(\tilde{\mathbf{x}})$$

由[[弱对偶性与强对偶性|弱对偶性]]：$g(\tilde{\boldsymbol{\lambda}}, \tilde{\boldsymbol{\nu}}) \leq d^* \leq p^* \leq f_0(\tilde{\mathbf{x}})$

因此 $f_0(\tilde{\mathbf{x}}) = g(\tilde{\boldsymbol{\lambda}}, \tilde{\boldsymbol{\nu}})$ ⇒ $d^* = p^* = f_0(\tilde{\mathbf{x}})$。

---

## 四、非凸情况下的KKT条件

### 4.1 一般问题

对一般可微优化问题（非凸），在**约束品性**（如LICQ）下：
- KKT是**必要条件**（局部最优 ⇒ KKT存在乘子）
- KKT + 凸性 ⇒ 全局最优

### 4.2 非凸时KKT的有效性

KKT点可能是：
- 全局最优解
- 局部最优解
- 鞍点
- 甚至局部极大值点

---

## 五、KKT条件的几何解释

### 5.1 法锥角度的解释

$\tilde{\mathbf{x}}$ 为 $\min_{\mathbf{x} \in \mathcal{F}} f_0(\mathbf{x})$ 的最优解 ⇔

$$-\nabla f_0(\tilde{\mathbf{x}}) \in \mathcal{N}_\mathcal{F}(\tilde{\mathbf{x}})$$

其中 $\mathcal{N}_\mathcal{F}$ 是可行域在 $\tilde{\mathbf{x}}$ 处的**法锥**（[[支撑超平面]]）。

在KKT条件下：
$$\mathcal{N}_\mathcal{F}(\tilde{\mathbf{x}}) = \left\{\sum_{i \in \mathcal{A}(\tilde{\mathbf{x}})} \lambda_i \nabla f_i(\tilde{\mathbf{x}}) + \sum_{j=1}^{p} \nu_j \nabla h_j(\tilde{\mathbf{x}}) \mid \lambda_i \geq 0\right\}$$

> 法锥由紧约束的梯度**非负**生成。

### 5.2 分离超平面视角

KKT条件等价于：紧约束梯度的锥**包含**负目标梯度，即存在一个超平面将"更好的方向"从可行方向中分离出来。

参见 [[分离超平面定理]]

---

## 六、KKT条件的计算应用

### 6.1 解析求解

对小规模问题，直接求解KKT方程组得到候选最优解。

**例**：$\min x_1^2 + x_2^2$ s.t. $x_1 + x_2 \geq 1$

写出KKT：$2x_1 - \lambda = 0$, $2x_2 - \lambda = 0$, $\lambda(1 - x_1 - x_2) = 0$, $\lambda \geq 0$。

解得 $x_1 = x_2 = 1/2$, $\lambda = 1$。

### 6.2 数值方法

[[有约束凸优化算法|内点法]]本质上是求解扰动版本 KKT 条件的Newton迭代。

### 6.3 验证最优性

给定任一候选解，检查KKT条件即可验证其是否为全局最优（凸问题下充要）。

---

## 七、互补松弛性的深入理解

$\lambda_i f_i(\mathbf{x}^*) = 0$ 意味着：

| 情况 | $\lambda_i^*$ | $f_i(\mathbf{x}^*)$ | 含义 |
|------|-------------|-------------------|------|
| 紧约束 | $> 0$ | $= 0$ | 约束限制最优解，有影子价格 |
| 紧约束 | $= 0$ | $= 0$ | 约束恰好触碰但不限制（退化） |
| 松弛约束 | $= 0$ | $< 0$ | 约束不影响最优解 |

---

## 相关笔记

- [[对偶理论]] — Lagrange对偶与KKT的等价性
- [[Slater条件与约束品性]] — KKT定理的前提条件
- [[Lagrange对偶函数]] — KKT中的乘子来自对偶
- [[凸函数一阶条件]] — 无约束最优性的KKT特例（$\nabla f = 0$）
- [[扰动与灵敏度分析]] — $\lambda_i^*$ 作为影子价格
