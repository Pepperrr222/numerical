# 二阶锥规划 (Second-Order Cone Programming, SOCP)

SOCP是[[线性规划 LP|LP]]和[[二次规划 QP|QP]]的推广，约束条件为"仿射函数的范数不大于另一个仿射函数"。

[[凸优化问题]] | [[二次约束二次规划 QCQP]] | [[半定规划 SDP]] | [[对偶锥与广义不等式]]

---

## 一、标准形式

$$\begin{aligned} \min_{\mathbf{x}} \quad & \mathbf{f}^T \mathbf{x} \\ \text{s.t.} \quad & \|\mathbf{A}_i \mathbf{x} + \mathbf{b}_i\|_2 \leq \mathbf{c}_i^T \mathbf{x} + d_i, \quad i = 1, \ldots, m \\ & \mathbf{F}\mathbf{x} = \mathbf{g} \end{aligned}$$

其中 $\mathbf{A}_i \in \mathbb{R}^{n_i \times n}$，$\mathbf{b}_i \in \mathbb{R}^{n_i}$，$\mathbf{c}_i \in \mathbb{R}^n$，$d_i \in \mathbb{R}$。

---

## 二、二阶锥 (Second-Order Cone / Lorentz Cone)

### 2.1 定义

$$\mathcal{Q}^n = \{(\mathbf{y}, t) \in \mathbb{R}^{n} \times \mathbb{R} \mid \|\mathbf{y}\|_2 \leq t\}$$

- 又称 **Lorentz锥**、**冰淇淋锥**（ice-cream cone）
- $\mathcal{Q}^n$ 是**自对偶**的[[对偶锥与广义不等式|真锥]]

### 2.2 几何形状

二维投影：$\{(y, t) \mid |y| \leq t, t \geq 0\}$（楔形）

三维投影：圆锥形（冰淇淋筒形状）

### 2.3 二阶锥约束的含义

$$\|\mathbf{A}\mathbf{x} + \mathbf{b}\|_2 \leq \mathbf{c}^T\mathbf{x} + d \iff (\mathbf{A}\mathbf{x} + \mathbf{b}, \mathbf{c}^T\mathbf{x} + d) \in \mathcal{Q}^{n_i+1}$$

即点 $(\mathbf{A}\mathbf{x}+\mathbf{b}, \mathbf{c}^T\mathbf{x}+d)$ 位于二阶锥内。

---

## 三、SOCP的一般形式

使用广义不等式记法：

$$\begin{aligned} \min \quad & \mathbf{f}^T \mathbf{x} \\ \text{s.t.} \quad & (\mathbf{A}_i \mathbf{x} + \mathbf{b}_i, \mathbf{c}_i^T \mathbf{x} + d_i) \preceq_{\mathcal{Q}^{n_i+1}} \mathbf{0} \\ & \mathbf{F}\mathbf{x} = \mathbf{g} \end{aligned}$$

对偶锥为 $\mathcal{Q}^{n_i+1}$ 自身（自对偶）。

---

## 四、可转化为SOCP的问题

### 4.1 QP → SOCP

$$\min \mathbf{q}^T\mathbf{x} \text{ s.t. } \mathbf{G}\mathbf{x} \preceq \mathbf{h}$$

若目标为二次型 $\frac{1}{2}\mathbf{x}^T\mathbf{P}\mathbf{x} + \mathbf{q}^T\mathbf{x}$，令 $\mathbf{P} = \mathbf{M}^T\mathbf{M}$（Cholesky分解）：

$$\min_{\mathbf{x},t} t \quad \text{s.t.} \quad \|\mathbf{M}\mathbf{x} + \mathbf{M}^{-T}\mathbf{q}\|_2 \leq t$$

（$\mathbf{P} \succ 0$ 时，否则用锥分解）

### 4.2 QCQP → SOCP

对每个二次约束 $\frac{1}{2}\mathbf{x}^T\mathbf{P}_i\mathbf{x} + \mathbf{q}_i^T\mathbf{x} + r_i \leq 0$（$\mathbf{P}_i \succeq 0$）：

$$\|\mathbf{P}_i^{1/2}\mathbf{x} + \frac{1}{2}\mathbf{P}_i^{-1/2}\mathbf{q}_i\|_2 \leq \left(\frac{1}{4}\mathbf{q}_i^T\mathbf{P}_i^{-1}\mathbf{q}_i - r_i\right)^{1/2}$$

即每个QCQP约束等价于一个SOC约束 ⇒ QCQP ⊂ SOCP。

### 4.3 $\ell_1$ 正则化问题 → SOCP

$$\min \|\mathbf{A}\mathbf{x} - \mathbf{b}\|_2 + \lambda\|\mathbf{x}\|_1$$

可以化为SOCP（引入辅助变量）。

### 4.4 鲁棒最小二乘 → SOCP

$$\min_{\mathbf{x}} \sup_{\|\Delta\mathbf{A}\|_F \leq \rho} \|(\mathbf{A} + \Delta\mathbf{A})\mathbf{x} - \mathbf{b}\|_2$$

可化为SOCP。

### 4.5 凸优化中的约束对

$$\frac{\|\mathbf{A}\mathbf{x} + \mathbf{b}\|_2^2}{\mathbf{c}^T\mathbf{x} + d} \leq t, \quad \mathbf{c}^T\mathbf{x} + d > 0$$

可化为SOC约束 $\left\|\begin{bmatrix} 2(\mathbf{A}\mathbf{x}+\mathbf{b}) \\ t - (\mathbf{c}^T\mathbf{x}+d) \end{bmatrix}\right\|_2 \leq t + (\mathbf{c}^T\mathbf{x}+d)$。

---

## 五、对偶SOCP

### 5.1 原SOCP的Lagrange对偶

Lagrange函数：$L(\mathbf{x}, \boldsymbol{\lambda}_i, \boldsymbol{\nu}) = \mathbf{f}^T\mathbf{x} + \sum \boldsymbol{\lambda}_i^T(\mathbf{A}_i\mathbf{x}+\mathbf{b}_i) - \sum \lambda_{i,t}(\mathbf{c}_i^T\mathbf{x}+d_i) + \boldsymbol{\nu}^T(\mathbf{F}\mathbf{x}-\mathbf{g})$

其中 $\boldsymbol{\lambda}_i = (\mathbf{z}_i, \lambda_{i,t})$ 满足 $\|\mathbf{z}_i\|_2 \leq \lambda_{i,t}$（对偶锥约束）。

**对偶SOCP**也是SOCP（自对偶性）。

### 5.2 强对偶

若存在严格可行点（Slater条件），SOCP具有**强对偶性**。

---

## 六、SOCP的扩展

### 6.1 旋转二阶锥

$$\mathcal{Q}_r^n = \{(\mathbf{y}, s, t) \in \mathbb{R}^{n-2} \times \mathbb{R} \times \mathbb{R} \mid \|\mathbf{y}\|_2^2 \leq 2st, s \geq 0, t \geq 0\}$$

旋转二阶锥与标准二阶锥通过线性变换等价：
$\|\mathbf{y}\|_2^2 \leq 2st \iff \left\|\begin{bmatrix} 2\mathbf{y} \\ s-t \end{bmatrix}\right\|_2 \leq s+t$

### 6.2 应用

旋转二阶锥可以便捷地表达：
- 二次型 $\mathbf{x}^T\mathbf{P}\mathbf{x} \leq t$ 当 $\mathbf{P} \succeq 0$
- 有理幂约束 $x^{3/2} \leq t$
- 几何规划的凸化形式

---

## 七、包含关系

```
LP ⊂ QP ⊂ QCQP ⊂ SOCP ⊂ SDP
```

SOCP在建模能力和计算效率之间取得了良好平衡：
- 建模能力强于QCQP（可处理更多非线性约束）
- 求解效率远高于[[半定规划 SDP|SDP]]（SDP涉及矩阵锥，计算成本高昂）

---

## 八、算法与求解

SOCP可通过以下方法求解：

1. **内点法**（见 [[有约束凸优化算法]]）
   - 基于二阶锥的自和谐障碍函数：$\phi(\mathbf{y}, t) = -\log(t^2 - \|\mathbf{y}\|_2^2)$
   - 复杂度：$O(\sqrt{m}\log(1/\epsilon))$

2. **ADMM**（见 [[近端梯度法与ADMM]]）
   - 对每个SOC约束引入辅助变量

3. 专用求解器：**ECOS**、**SCS**、**MOSEK**、**Gurobi**

---

## 相关笔记

- [[凸优化问题]] — 凸优化总览
- [[线性规划 LP]] — SOCP包含LP
- [[二次约束二次规划 QCQP]] — QCQP可转化为SOCP
- [[半定规划 SDP]] — SDP推广SOCP
- [[对偶锥与广义不等式]] — 二阶锥的锥性质
- [[有约束凸优化算法]] — 内点法求解SOCP
