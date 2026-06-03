# 二次规划 (Quadratic Programming, QP)

二次规划是目标函数为二次型、约束为仿射的[[凸优化问题]]。它是仅次于[[线性规划 LP|LP]]的第二大类优化问题。

[[凸优化问题]] | [[线性规划 LP]] | [[二次约束二次规划 QCQP]] | [[二阶锥规划 SOCP]]

---

## 一、标准形式

$$\begin{aligned} \min_{\mathbf{x}} \quad & \frac{1}{2}\mathbf{x}^T \mathbf{P}\mathbf{x} + \mathbf{q}^T \mathbf{x} + r \\ \text{s.t.} \quad & \mathbf{G}\mathbf{x} \preceq \mathbf{h} \\ & \mathbf{A}\mathbf{x} = \mathbf{b} \end{aligned}$$

- $\mathbf{P} \in \mathbf{S}_+^n$（半正定）
- $\mathbf{q} \in \mathbb{R}^n$, $r \in \mathbb{R}$
- $\mathbf{G} \in \mathbb{R}^{m \times n}$, $\mathbf{A} \in \mathbb{R}^{p \times n}$

**$\mathbf{P} \succeq 0$ 保证凸性**；$\mathbf{P} = \mathbf{0}$ 退化为 [[线性规划 LP|LP]]。

---

## 二、几何结构

### 2.1 目标函数

$$f_0(\mathbf{x}) = \frac{1}{2}\mathbf{x}^T \mathbf{P}\mathbf{x} + \mathbf{q}^T \mathbf{x} + r$$

- 梯度：$\nabla f_0(\mathbf{x}) = \mathbf{P}\mathbf{x} + \mathbf{q}$
- Hessian：$\nabla^2 f_0(\mathbf{x}) = \mathbf{P} \succeq 0$

目标函数的等高线是**椭球**（或圆柱/退化椭球）。

### 2.2 可行域

$\mathcal{F} = \{\mathbf{x} \mid \mathbf{G}\mathbf{x} \preceq \mathbf{h}, \mathbf{A}\mathbf{x} = \mathbf{b}\}$ 是**多面体**。

几何直观：在**多面体**上找一个点，使其到某个椭球中心的"马氏距离"最小。

---

## 三、无约束QP

$$\min_{\mathbf{x}} \quad \frac{1}{2}\mathbf{x}^T \mathbf{P}\mathbf{x} + \mathbf{q}^T \mathbf{x} + r$$

### 3.1 解析解

当 $\mathbf{P} \succ 0$：

$$\mathbf{x}^* = -\mathbf{P}^{-1}\mathbf{q}$$

$$p^* = r - \frac{1}{2}\mathbf{q}^T \mathbf{P}^{-1}\mathbf{q}$$

### 3.2 $\mathbf{P} \succeq 0$ 但奇异的情况

- 若 $\mathbf{q} \in \text{range}(\mathbf{P})$，存在解（但不唯一）
- 若 $\mathbf{q} \notin \text{range}(\mathbf{P})$，问题无下界

---

## 四、等式约束QP（KKT系统）

$$\begin{aligned} \min \quad & \frac{1}{2}\mathbf{x}^T \mathbf{P}\mathbf{x} + \mathbf{q}^T \mathbf{x} \\ \text{s.t.} \quad & \mathbf{A}\mathbf{x} = \mathbf{b} \end{aligned}$$

### 4.1 KKT条件

$$\begin{bmatrix} \mathbf{P} & \mathbf{A}^T \\ \mathbf{A} & \mathbf{0} \end{bmatrix} \begin{bmatrix} \mathbf{x}^* \\ \boldsymbol{\nu}^* \end{bmatrix} = \begin{bmatrix} -\mathbf{q} \\ \mathbf{b} \end{bmatrix}$$

该矩阵称为 **KKT矩阵**（鞍点矩阵）。

### 4.2 KKT矩阵的非奇异性

若 $\mathbf{P} \succ 0$ 或 $\mathbf{A}$ 行满秩，KKT矩阵非奇异 ⇒ 唯一解。

---

## 五、不等式约束QP

$$\min \quad \frac{1}{2}\mathbf{x}^T \mathbf{P}\mathbf{x} + \mathbf{q}^T \mathbf{x} \quad \text{s.t.} \quad \mathbf{G}\mathbf{x} \preceq \mathbf{h}$$

[[KKT条件]]：

$$\begin{aligned} \mathbf{P}\mathbf{x}^* + \mathbf{q} + \mathbf{G}^T \boldsymbol{\lambda}^* &= \mathbf{0} \\ \mathbf{G}\mathbf{x}^* &\preceq \mathbf{h} \\ \boldsymbol{\lambda}^* &\succeq \mathbf{0} \\ \lambda_i^* (\mathbf{g}_i^T \mathbf{x}^* - h_i) &= 0, \quad \forall i \end{aligned}$$

### 5.1 求解方法

- **Active-set方法**：猜测哪些约束是紧的，求解等式约束子问题
- **内点法**：对数障碍函数
- **算子分裂法**：转化为变分不等式求解

---

## 六、对偶QP

原QP的Lagrange对偶也是QP。

### 原问题

$$\begin{aligned} \min \quad & \frac{1}{2}\mathbf{x}^T \mathbf{P}\mathbf{x} + \mathbf{q}^T \mathbf{x} \\ \text{s.t.} \quad & \mathbf{G}\mathbf{x} \preceq \mathbf{h} \end{aligned}$$

### Lagrange函数

$$L(\mathbf{x}, \boldsymbol{\lambda}) = \frac{1}{2}\mathbf{x}^T \mathbf{P}\mathbf{x} + \mathbf{q}^T \mathbf{x} + \boldsymbol{\lambda}^T (\mathbf{G}\mathbf{x} - \mathbf{h})$$

### 对偶函数（最小化 $\mathbf{x}$）

对 $\mathbf{x}$ 求最小：$\mathbf{P}\mathbf{x} + \mathbf{q} + \mathbf{G}^T \boldsymbol{\lambda} = 0$ ⇒ $\mathbf{x} = -\mathbf{P}^{-1}(\mathbf{q} + \mathbf{G}^T \boldsymbol{\lambda})$

代入得到对偶QP：

$$\begin{aligned} \max \quad & -\frac{1}{2}\boldsymbol{\lambda}^T \mathbf{G}\mathbf{P}^{-1}\mathbf{G}^T \boldsymbol{\lambda} - (\mathbf{h}^T + \mathbf{q}^T \mathbf{P}^{-1}\mathbf{G}^T)\boldsymbol{\lambda} - \frac{1}{2}\mathbf{q}^T \mathbf{P}^{-1}\mathbf{q} \\ \text{s.t.} \quad & \boldsymbol{\lambda} \succeq 0 \end{aligned}$$

这也是一个QP（在 $\boldsymbol{\lambda} \succeq 0$ 约束下）。

---

## 七、典型应用

### 7.1 最小二乘

$$\min_{\mathbf{x}} \|\mathbf{A}\mathbf{x} - \mathbf{b}\|_2^2 = \mathbf{x}^T (\mathbf{A}^T\mathbf{A})\mathbf{x} - 2\mathbf{b}^T\mathbf{A}\mathbf{x} + \mathbf{b}^T\mathbf{b}$$

$\mathbf{P} = 2\mathbf{A}^T\mathbf{A} \succeq 0$ ⇒ QP。

### 7.2 LASSO（可转化为QP）

$$\min \|\mathbf{A}\mathbf{x} - \mathbf{b}\|_2^2 + \lambda\|\mathbf{x}\|_1$$

$\ell_1$ 项可拆分为 $\mathbf{x} = \mathbf{x}^+ - \mathbf{x}^-$（$\mathbf{x}^+, \mathbf{x}^- \succeq 0$），化为QP。

### 7.3 Markowitz投资组合优化

$$\begin{aligned} \min \quad & \mathbf{x}^T \Sigma \mathbf{x} \\ \text{s.t.} \quad & \boldsymbol{\mu}^T \mathbf{x} \geq R_{\min} \\ & \mathbf{1}^T \mathbf{x} = 1, \quad \mathbf{x} \succeq 0 \end{aligned}$$

$\Sigma \succeq 0$（协方差矩阵），此为QP。

### 7.4 支持向量机（SVM）

$$\begin{aligned} \min \quad & \frac{1}{2}\|\mathbf{w}\|_2^2 \\ \text{s.t.} \quad & y_i(\mathbf{w}^T \mathbf{x}_i + b) \geq 1 \end{aligned}$$

或其对偶形式（也是QP）。

---

## 八、QP → SOCP 转化

任何QP可转化为 [[二阶锥规划 SOCP|SOCP]]：

$$\min \mathbf{q}^T \mathbf{x} \quad \text{subject to} \quad \|\mathbf{P}^{1/2}\mathbf{x} + \mathbf{P}^{-1/2}\mathbf{q}\|_2 \leq t, \mathbf{G}\mathbf{x} \preceq \mathbf{h}$$

（$\mathbf{P} \succ 0$ 时）

这说明 QP ⊂ SOCP。

---

## 相关笔记

- [[线性规划 LP]] — QP的特例（$\mathbf{P}=0$）
- [[二次约束二次规划 QCQP]] — 二次约束推广
- [[二阶锥规划 SOCP]] — 锥推广
- [[对偶理论]] — QP对偶理论
- [[KKT条件]] — QP的KKT系统
- [[有约束凸优化算法]] — 求解方法（Active-set, 内点法）
