# Slater条件与约束品性

**约束品性**（Constraint Qualification, CQ）是保证[[弱对偶性与强对偶性|强对偶]]成立的技术条件。其中，[[Slater条件与约束品性|Slater条件]]是凸优化中最简单实用的约束品性。

[[对偶理论]] | [[弱对偶性与强对偶性]] | [[KKT条件]]

---

## 一、为什么需要约束品性？

凸优化问题一般具有强对偶性，但**不是自动的**。需要在**可行域内部**至少有一个严格可行点来保证。

**反例**（无强对偶的凸问题）：

$$\begin{aligned} \min \quad & e^{-x_2} \\ \text{s.t.} \quad & x_1^2 + x_2^2 - x_1 \leq 0 \\ & x_2 \leq 0 \end{aligned}$$

（可行域只有点 $\mathbf{x} = (0,0)$ 和 $(1,0)$，没有严格可行点，强对偶不成立）

---

## 二、Slater条件（凸优化专用）

### 2.1 标准Slater条件

对于凸优化问题（$f_i$ 凸，$h_i$ 仿射），若存在 $\tilde{\mathbf{x}} \in \text{relint }\mathcal{D}$ 使得：

$$f_i(\tilde{\mathbf{x}}) < 0, \quad i = 1, \ldots, m$$
$$\mathbf{A}\tilde{\mathbf{x}} = \mathbf{b}$$

则**强对偶成立**（$d^* = p^*$），且若 $d^* > -\infty$，对偶最优值可达到。

### 2.2 放松的Slater条件

若前 $k$ 个不等式约束 $f_1, \ldots, f_k$ 是**仿射函数**，则Slater条件放松为：

$$f_i(\tilde{\mathbf{x}}) \leq 0, \quad i = 1, \ldots, k \quad \text{(仿射约束，可等号)}$$
$$f_i(\tilde{\mathbf{x}}) < 0, \quad i = k+1, \ldots, m \quad \text{(非仿射凸约束，严格不等式)}$$

**关键推论**：
- **线性规划（LP）**：Slater条件简化为仅要求**可行性**。若LP可行，强对偶成立。
- **二次规划（QP）**：若所有约束为线性，同上。

### 2.3 Slater条件的几何意义

存在 $\tilde{\mathbf{x}}$ 位于**所有不等式约束的严格内部**和等式约束的仿射平面上。

等价：可行域 $\mathcal{F}$ 的**相对内部非空**。

---

## 三、一般非线性规划的约束品性

（对非凸问题或一般光滑问题）

### 3.1 LICQ（线性独立约束品性）

在 $\mathbf{x}^*$ 处，**紧约束**的梯度线性无关：

$$\{\nabla f_i(\mathbf{x}^*) \mid i \in \mathcal{A}(\mathbf{x}^*)\} \cup \{\nabla h_i(\mathbf{x}^*) \mid i=1,\ldots,p\} \text{ 线性无关}$$

其中 $\mathcal{A}(\mathbf{x}) = \{i \mid f_i(\mathbf{x}) = 0\}$ 是紧约束（active constraints）指标集。

### 3.2 MFCQ（Mangasarian-Fromovitz约束品性）

在 $\mathbf{x}^*$ 处：
- $\{\nabla h_i(\mathbf{x}^*)\}$ 线性无关
- 存在 $\mathbf{d}$ 使得 $\nabla f_i(\mathbf{x}^*)^T \mathbf{d} < 0$（对所有紧不等式约束）且 $\nabla h_i(\mathbf{x}^*)^T \mathbf{d} = 0$

MFCQ比LICQ弱（更容易满足）。

### 3.3 Abadie约束品性

要求**切线锥** = **线性化可行方向锥**。

$$\mathcal{T}_\mathcal{F}(\mathbf{x}^*) = \mathcal{L}_\mathcal{F}(\mathbf{x}^*)$$

这是最优性必要条件的最弱约束品性。

### 3.4 包含关系

$$LICQ \Rightarrow MFCQ \Rightarrow Abadie$$

Slater 条件 ≈ MFCQ 在凸优化中的类比（针对全局强对偶，而非仅局部最优性）。

---

## 四、具体问题的强对偶条件

| 问题类型 | 强对偶条件 | 复杂性 |
|---------|-----------|--------|
| LP | 任意一方可行 | 自动 |
| QP | 可行（因为线性约束） | 宽松 |
| QCQP（凸） | 存在严格可行点 | 通常满足 |
| SOCP | 存在严格可行点（SOC内部） | 通常满足 |
| SDP | 存在严格可行点（如 $\mathbf{X} \succ 0$） | 需验证 |
| 一般凸优化 | Slater条件 | 需构造或验证 |
| 非凸优化 | 无通用条件（通常 $d^* < p^*$） | 有对偶间隙 |

---

## 五、Slater条件的验证

### 5.1 判定方法

1. 寻找一个点 $\tilde{\mathbf{x}}$ 满足所有约束（包括strict inequalities for nonlinear ones）
2. 证明这样的点存在（如通过构造、连续性论证、或凸性保证）

### 5.2 实际验证

通常可以通过求解一个**可行性问题**来验证：

$$\text{Find } \mathbf{x} \text{ s.t. } f_i(\mathbf{x}) < 0, \mathbf{A}\mathbf{x} = \mathbf{b}$$

或最大化"最小松弛"：$\max_{\mathbf{x}, s} s$ s.t. $f_i(\mathbf{x}) + s \leq 0, \mathbf{A}\mathbf{x} = \mathbf{b}$，检查 $s^* > 0$。

---

## 六、关于约束品性

### 6.1 Slater条件是**充分非必要**条件

有些凸问题不满足Slater条件但仍然有强对偶。Slater条件对凸问题来说是最简单实用的**充分条件**。

### 6.2 凸优化的约束品性

由于 $f_i$ 凸，$h_i$ 仿射，约束集合 $\mathcal{F}$ 是凸集。Slater条件确保 $\mathcal{F}$ 的**相对内部**非空，这保证了在最优值 $p^*$ 处的几何分离可以实现。

### 6.3 等价的几何观点

Slater条件成立 ⇔ 在 $p^*$ 处的上镜图集合 $\mathcal{G}$ 与原点的分离是通过**非垂直超平面**。

这是在凸性假设下保证[[分离超平面定理|分离超平面]]存在的关键。

---

## 相关笔记

- [[对偶理论]] — 对偶理论总览
- [[弱对偶性与强对偶性]] — 强对偶的定义
- [[KKT条件]] — 约束品性在最优性条件中的作用
- [[Lagrange对偶函数]] — 对偶函数的构造
- [[凸优化问题标准形式]] — 凸优化问题的建模
