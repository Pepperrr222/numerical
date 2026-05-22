# 线性规划 (Linear Programming, LP)

[[线性规划 LP|线性规划]]是最简单的[[凸优化问题]]，目标和约束均为**仿射函数**。所有线性规划的可行域为**多面体**。

[[凸优化问题]] | [[对偶理论]] | [[二次规划 QP]]

---

## 一、标准形式

### 1.1 不等式形式

$$\begin{aligned} \min_{\mathbf{x}} \quad & \mathbf{c}^T \mathbf{x} \\ \text{s.t.} \quad & \mathbf{A}\mathbf{x} \preceq \mathbf{b} \end{aligned}$$

### 1.2 标准形式（Standard Form）

$$\begin{aligned} \min_{\mathbf{x}} \quad & \mathbf{c}^T \mathbf{x} \\ \text{s.t.} \quad & \mathbf{A}\mathbf{x} = \mathbf{b} \\ & \mathbf{x} \succeq 0 \end{aligned}$$

其中 $\mathbf{A} \in \mathbb{R}^{m \times n}$，$\mathbf{b} \in \mathbb{R}^m$，$\mathbf{c} \in \mathbb{R}^n$。

### 1.3 两种形式的转换

- 不等式 → 标准形式：引入松弛变量 $\mathbf{s} \succeq 0$，$\mathbf{A}\mathbf{x} + \mathbf{s} = \mathbf{b}$
- 标准形式 → 不等式：$\mathbf{A}\mathbf{x} = \mathbf{b} \iff \mathbf{A}\mathbf{x} \leq \mathbf{b}, \mathbf{A}\mathbf{x} \geq \mathbf{b}$
- 自由变量 $x_i$ → 非负变量：$x_i = x_i^+ - x_i^-$（$x_i^+, x_i^- \geq 0$）

---

## 二、几何结构

### 2.1 可行域为多面体

标准形式的可行域：$\mathcal{P} = \{\mathbf{x} \mid \mathbf{A}\mathbf{x} = \mathbf{b}, \mathbf{x} \succeq 0\}$ 是一个**多面体**。

### 2.2 顶点与基本可行解

多面体的**顶点**（极端点）对应于**基本可行解**（BFS）：

- 选取 $\mathbf{A}$ 的 $m$ 列作为基矩阵 $\mathbf{B}$
- $\mathbf{x}_B = \mathbf{B}^{-1}\mathbf{b} \succeq 0$，$\mathbf{x}_N = 0$
- 基本定理：若 LP 有有限最优值，必存在一个**顶点最优解**

### 2.3 最优值的可能情况

1. **不可行**：$\mathcal{P} = \varnothing$
2. **无下界**：存在 $\mathbf{d}$ 使得 $\mathbf{A}\mathbf{d} = 0$，$\mathbf{d} \succeq 0$ 且 $\mathbf{c}^T \mathbf{d} < 0$（无界方向）
3. **有限最优值**：最优值在顶点处取得

---

## 三、对偶性

### 3.1 LP对偶

**原问题**：
$$\begin{aligned} \min \quad & \mathbf{c}^T \mathbf{x} \\ \text{s.t.} \quad & \mathbf{A}\mathbf{x} = \mathbf{b} \\ & \mathbf{x} \succeq 0 \end{aligned}$$

**对偶问题**：
$$\begin{aligned} \max \quad & \mathbf{b}^T \mathbf{y} \\ \text{s.t.} \quad & \mathbf{A}^T \mathbf{y} \preceq \mathbf{c} \end{aligned}$$

或等价形式（引入对偶松弛 $\mathbf{s} = \mathbf{c} - \mathbf{A}^T\mathbf{y} \succeq 0$）：

$$\begin{aligned} \max \quad & \mathbf{b}^T \mathbf{y} \\ \text{s.t.} \quad & \mathbf{A}^T \mathbf{y} + \mathbf{s} = \mathbf{c} \\ & \mathbf{s} \succeq 0 \end{aligned}$$

### 3.2 强对偶性

**LP的强对偶定理**：若原问题（或对偶问题）可行，则 $p^* = d^*$。

LP不需要Slater条件即可保证强对偶（因为约束是仿射的）。

### 3.3 互补松弛性

$$\mathbf{x}^* \succeq 0, \quad \mathbf{s}^* \succeq 0, \quad x_i^* s_i^* = 0, \quad i = 1,\ldots,n$$

---

## 四、LP的四种可能

| 原问题 \ 对偶问题 | 可行有界 | 可行无界 | 不可行 |
|-----------------|---------|---------|--------|
| 可行有界 | ✓ | ✗ | ✗ |
| 可行无界 | ✗ | ✗ | ✓ |
| 不可行 | ✗ | ✓ | ✓ |

（"✓" = 可能出现，"✗" = 不可能出现）

---

## 五、经典算法

### 5.1 单纯形法 (Simplex Method)

- **思想**：沿多面体的**棱**从一个顶点移动到相邻的更优顶点
- **复杂度**：理论上指数（Klee-Minty反例），实践中**极其高效**
- **优势**：可热启动（warm start），适合灵敏度分析

### 5.2 内点法 (Interior Point Method)

- **思想**：通过障碍函数在多面体内部追踪中心路径
- **复杂度**：多项式 $O(\sqrt{n} \log(1/\epsilon))$
- **优势**：对大规模稀疏问题有优势

### 5.3 具体方法

- **原始单纯形法**（Primal Simplex）
- **对偶单纯形法**（Dual Simplex）
- **原始-对偶路径跟随内点法**

参见 [[有约束凸优化算法]]

---

## 六、灵敏度分析

LP的最优值对约束右端项的灵敏度：

$$\frac{\partial p^*}{\partial b_i} \approx \nu_i^*$$

其中 $\boldsymbol{\nu}^*$ 是对偶最优解（影子价格）。

类似地，对目标系数的灵敏度：$\frac{\partial p^*}{\partial c_j} \approx x_j^*$。

---

## 七、常见应用

| 应用 | 描述 |
|------|------|
| 运输问题 | 最小化运输成本 |
| 分配问题 | 任务与资源的匹配 |
| 网络流 | 最大流、最小费用流 |
| 生产调度 | 资源约束下最大化产出 |
| 投资组合 | $\ell_1$ 或 $\ell_\infty$ 风险度量 |
| 压缩感知 | $\ell_1$ 极小化（可通过LP求解） |
| 博弈论 | 零和博弈的minimax解 |

---

## 相关笔记

- [[凸优化问题]] — 凸优化总览
- [[二次规划 QP]] — 二次目标推广
- [[对偶理论]] — Lagrange对偶的特殊情况
- [[有约束凸优化算法]] — 内点法
- [[凸优化问题标准形式]] — 问题建模框架
