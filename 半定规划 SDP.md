# 半定规划 (Semidefinite Programming, SDP)

SDP是[[凸优化问题]]的最一般形式之一，在**半正定锥**上优化线性目标。它为组合优化、控制论、矩阵分析提供了统一框架。

[[凸优化问题]] | [[二阶锥规划 SOCP]] | [[对偶锥与广义不等式]] | [[二次约束二次规划 QCQP]]

---

## 一、标准形式

### 1.1 不等式形式

$$\begin{aligned} \min_{\mathbf{x}} \quad & \mathbf{c}^T \mathbf{x} \\ \text{s.t.} \quad & \mathbf{F}_0 + \sum_{i=1}^{n} x_i \mathbf{F}_i \preceq \mathbf{0} \end{aligned}$$

其中 $\mathbf{F}_i \in \mathbf{S}^k$（$k \times k$ 对称矩阵）。

约束 $\mathbf{F}(\mathbf{x}) \preceq \mathbf{0}$ = **线性矩阵不等式（LMI）**，含义是 $-\mathbf{F}(\mathbf{x}) \succeq 0$。

### 1.2 标准对偶形式

$$\begin{aligned} \max_{\mathbf{X}} \quad & -\text{tr}(\mathbf{F}_0 \mathbf{X}) \\ \text{s.t.} \quad & \text{tr}(\mathbf{F}_i \mathbf{X}) = c_i, \quad i=1,\ldots,n \\ & \mathbf{X} \succeq 0 \end{aligned}$$

其中 $\mathbf{X} \in \mathbf{S}_+^k$。

### 1.3 多LMI形式

$$\begin{aligned} \min \quad & \mathbf{c}^T \mathbf{x} \\ \text{s.t.} \quad & \mathbf{F}^{(j)}(\mathbf{x}) \preceq \mathbf{0}, \quad j=1,\ldots,r \end{aligned}$$

等价于单LMI（块对角形式）：

$$\text{diag}(\mathbf{F}^{(1)}(\mathbf{x}), \ldots, \mathbf{F}^{(r)}(\mathbf{x})) \preceq \mathbf{0}$$

---

## 二、半正定锥的几何

### 2.1 $\mathbf{S}_+^k$ 的性质

- $\mathbf{S}_+^k = \{\mathbf{X} \in \mathbf{S}^k \mid \mathbf{X} \succeq 0\}$
- 是 $\mathbf{S}^k$ 中的**真锥**：闭、尖、实心
- **自对偶**：$(\mathbf{S}_+^k)^* = \mathbf{S}_+^k$
- $\text{int }\mathbf{S}_+^k = \mathbf{S}_{++}^k$（正定矩阵）

### 2.2 二阶锥与半正定锥的关系

$$\mathcal{Q}^3 = \{(x,y,t) \mid \sqrt{x^2+y^2} \leq t\}$$

等价于：

$$\begin{bmatrix} t & x & y \\ x & t & 0 \\ y & 0 & t \end{bmatrix} \succeq 0$$

**SOCP是SDP的特例**（精确在矩阵的某个特定子空间上）。

---

## 三、SDP的对偶性

### 3.1 原问题（Primal）

$$\begin{aligned} \min_{\mathbf{x}} \quad & \mathbf{c}^T \mathbf{x} \\ \text{s.t.} \quad & \sum x_i \mathbf{F}_i + \mathbf{F}_0 \preceq \mathbf{0} \end{aligned}$$

### 3.2 对偶问题（Dual）

$$\begin{aligned} \max_{\mathbf{Z}} \quad & \text{tr}(\mathbf{F}_0 \mathbf{Z}) \\ \text{s.t.} \quad & \text{tr}(\mathbf{F}_i \mathbf{Z}) = -c_i, \quad i=1,\ldots,n \\ & \mathbf{Z} \succeq 0 \end{aligned}$$

### 3.3 弱对偶与强对偶

- **弱对偶**恒成立：$d^* \leq p^*$
- **强对偶**：若存在严格原始可行解（$\sum x_i\mathbf{F}_i + \mathbf{F}_0 \prec \mathbf{0}$），则 $d^* = p^*$ 且对偶最优值可达到

---

## 四、可转化为SDP的问题

### 4.1 线性规划 LP ⊂ SDP

$$\mathbf{A}\mathbf{x} \preceq \mathbf{b} \iff \text{diag}(\mathbf{b} - \mathbf{A}\mathbf{x}) \succeq 0$$

对角线上的非负约束 = 对角矩阵的半正定性。

### 4.2 SOCP ⊂ SDP

$$\|\mathbf{y}\|_2 \leq t \iff \begin{bmatrix} t\mathbf{I} & \mathbf{y} \\ \mathbf{y}^T & t \end{bmatrix} \succeq 0$$

（Schur补引理）

### 4.3 特征值优化

$$\begin{aligned} \min \quad & \lambda_{\max}(\mathbf{A}(\mathbf{x})) \\ \iff \min \quad & t \\ \text{s.t.} \quad & \mathbf{A}(\mathbf{x}) \preceq t\mathbf{I} \end{aligned}$$

（当 $\mathbf{A}(\mathbf{x})$ 为 $\mathbf{x}$ 的仿射函数时，是SDP）

### 4.4 矩阵范数最小化

$$\min \|\mathbf{A}(\mathbf{x})\|_2 \iff \min t \text{ s.t. } \begin{bmatrix} t\mathbf{I} & \mathbf{A}(\mathbf{x}) \\ \mathbf{A}(\mathbf{x})^T & t\mathbf{I} \end{bmatrix} \succeq 0$$

### 4.5 核范数/迹范数

$$\|\mathbf{X}\|_* = \sum \sigma_i(\mathbf{X}) \leq t \iff \exists \mathbf{U}, \mathbf{V}: \begin{bmatrix} \mathbf{U} & \mathbf{X} \\ \mathbf{X}^T & \mathbf{V} \end{bmatrix} \succeq 0, \text{tr}(\mathbf{U}) + \text{tr}(\mathbf{V}) \leq 2t$$

这可用于**矩阵补全**等问题。

---

## 五、经典应用

### 5.1 Lyapunov稳定性分析（控制论）

连续系统 $\dot{\mathbf{x}} = \mathbf{A}\mathbf{x}$ 稳定 ⇔ 存在 $\mathbf{P} \succ 0$ 使得：

$$\mathbf{A}^T\mathbf{P} + \mathbf{P}\mathbf{A} \prec 0$$

这是SDP可行性问题。

### 5.2 最大割（Max-Cut）的SDP松弛

$$\max \quad \sum w_{ij} \frac{1 - v_{ij}}{2} \quad \text{s.t.} \quad \mathbf{V} \succeq 0, v_{ii} = 1$$

Goemans-Williamson算法达到 0.878 近似比。

### 5.3 投资组合鲁棒优化

$$\min_{\mathbf{x}} \max_{\Sigma \in \mathcal{S}} \mathbf{x}^T \Sigma \mathbf{x} \quad \text{s.t.} \quad \boldsymbol{\mu}^T\mathbf{x} \geq R, \mathbf{1}^T\mathbf{x} = 1$$

当不确定集 $\mathcal{S}$ 用线性矩阵不等式描述时，化为SDP。

### 5.4 矩阵补全 (Matrix Completion)

已知部分元素，恢复低秩矩阵：

$$\min \|\mathbf{X}\|_* \text{ s.t. } X_{ij} = M_{ij}, (i,j) \in \Omega$$

核范数松弛化为SDP。

---

## 六、SDP的求解

### 6.1 内点法

- 利用半正定锥的**对数行列式障碍**：$\phi(\mathbf{X}) = -\log\det\mathbf{X}$
- 复杂度：对 $m$ 个约束的 $k \times k$ SDP，每步 $O(k^3 + mk^2)$
- 实际限制：$k \leq 2000$ 左右时内点法可用

### 6.2 一阶方法

- **ADMM**（见 [[近端梯度法与ADMM]]）：每次迭代投影到 $\mathbf{S}_+^k$（特征值分解 + 截断负特征值）
- **HOMOTOPY / 增广Lagrange法**

### 6.3 专用求解器

- **SDPT3**、**SeDuMi**、**CSDP** — 经典内点法
- **SCS** — 一阶算子分裂法
- **MOSEK** — 商业求解器

---

## 七、SDP的层次结构

```
LP ────→ QP ────→ QCQP ────→ SOCP ────→ SDP
                                          │
                                   ┌──────┴──────┐
                                   │              │
                              SOS优化         矩问题
                          (多项式优化)    (概率/统计)
```

SDP是**多项式优化**和**矩问题**的通用框架（通过平方和（SOS）表示和矩矩阵）。

---

## 附：Schur补引理

$$\begin{bmatrix} \mathbf{A} & \mathbf{B} \\ \mathbf{B}^T & \mathbf{C} \end{bmatrix} \succeq 0 \iff \mathbf{C} \succeq 0, \mathbf{A} - \mathbf{B}\mathbf{C}^\dagger \mathbf{B}^T \succeq 0, (\mathbf{I} - \mathbf{C}\mathbf{C}^\dagger)\mathbf{B} = \mathbf{0}$$

其中 $\mathbf{C}^\dagger$ 为Moore-Penrose伪逆。

当 $\mathbf{C} \succ 0$ 时简化为：$\mathbf{A} - \mathbf{B}\mathbf{C}^{-1}\mathbf{B}^T \succeq 0$。

---

## 相关笔记

- [[凸优化问题]] — 凸优化总览
- [[二阶锥规划 SOCP]] — SDP包含SOCP
- [[对偶锥与广义不等式]] — 半正定锥的锥性质
- [[对偶理论]] — SDP的对偶理论
- [[有约束凸优化算法]] — 内点法
- [[几何规划]] — 特殊凸优化问题的多项式化
