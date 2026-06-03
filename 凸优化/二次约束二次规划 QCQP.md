# 二次约束二次规划 (Quadratically Constrained Quadratic Programming, QCQP)

QCQP是[[二次规划 QP|QP]]的直接推广，约束也可以是二次型。QCQP包含了许多重要的优化问题作为特例。

[[凸优化问题]] | [[二次规划 QP]] | [[二阶锥规划 SOCP]] | [[半定规划 SDP]]

---

## 一、标准形式

$$\begin{aligned} \min_{\mathbf{x}} \quad & \frac{1}{2}\mathbf{x}^T \mathbf{P}_0 \mathbf{x} + \mathbf{q}_0^T \mathbf{x} + r_0 \\ \text{s.t.} \quad & \frac{1}{2}\mathbf{x}^T \mathbf{P}_i \mathbf{x} + \mathbf{q}_i^T \mathbf{x} + r_i \leq 0, \quad i=1,\ldots,m \\ & \mathbf{A}\mathbf{x} = \mathbf{b} \end{aligned}$$

- $\mathbf{P}_i \in \mathbf{S}_{+}^n$（若要求凸QCQP）
- 若 $\mathbf{P}_i = 0$ 对所有 $i \geq 1$：退化为 [[二次规划 QP|QP]]
- 若 $\mathbf{P}_0 = 0$ 且 $\mathbf{P}_i = 0$ 对所有 $i$：退化为 [[线性规划 LP|LP]]

---

## 二、凸QCQP vs 非凸QCQP

| 类型 | $\mathbf{P}_0$ | $\mathbf{P}_i$（$i \geq 1$）| 可解性 |
|------|---------------|------------------------|--------|
| 凸QCQP | $\succeq 0$ | $\succeq 0$ | 全局最优，多项式时间 |
| 非凸QCQP | 不定 | 任意 | 一般NP-hard |

---

## 三、凸QCQP的几何

### 3.1 可行域

每个二次约束定义了一个**椭球**（或退化椭球/抛物面/双曲面片）：

$$\{\mathbf{x} \mid \frac{1}{2}\mathbf{x}^T \mathbf{P}\mathbf{x} + \mathbf{q}^T \mathbf{x} + r \leq 0\}$$

当 $\mathbf{P} \succ 0$ 时，这是一个实心椭球（可写为 $(\mathbf{x} - \mathbf{x}_c)^T \mathbf{P}^{-1}(\mathbf{x} - \mathbf{x}_c) \leq \rho$）。

### 3.2 可行域 = 椭球的交

凸QCQP的可行域是**多个椭球的交集**（加上仿射等式约束）。

### 3.3 等高线

目标函数的等高线也是椭球。

---

## 四、QCQP → SOCP 转化

对 $\mathbf{P} \succeq 0$（例如 $\mathbf{P} = \mathbf{M}^T\mathbf{M}$），二次不等式：

$$\frac{1}{2}\mathbf{x}^T \mathbf{P}\mathbf{x} + \mathbf{q}^T \mathbf{x} + r \leq 0$$

等价于二阶锥约束：

$$\left\|\begin{bmatrix} \frac{1}{\sqrt{2}}\mathbf{M}\mathbf{x} \\ \frac{1}{2}(\mathbf{q}^T\mathbf{x} + r + 1) \end{bmatrix}\right\|_2 \leq \frac{1}{2}(1 - \mathbf{q}^T\mathbf{x} - r)$$

> 这建立了 **凸QCQP ⊆ SOCP** 的关系。

---

## 五、非凸QCQP的处理方法

### 5.1 SDP松弛

将 $\mathbf{X} = \mathbf{x}\mathbf{x}^T$ 松弛为 $\mathbf{X} \succeq \mathbf{x}\mathbf{x}^T$：

$$\begin{aligned} \min_{\mathbf{x}, \mathbf{X}} \quad & \frac{1}{2}\text{tr}(\mathbf{P}_0\mathbf{X}) + \mathbf{q}_0^T \mathbf{x} + r_0 \\ \text{s.t.} \quad & \frac{1}{2}\text{tr}(\mathbf{P}_i\mathbf{X}) + \mathbf{q}_i^T \mathbf{x} + r_i \leq 0 \\ & \begin{bmatrix} \mathbf{X} & \mathbf{x} \\ \mathbf{x}^T & 1 \end{bmatrix} \succeq 0 \end{aligned}$$

### 5.2 秩1近似

SDP松弛给出下界。若解满足 $\text{rank}(\mathbf{X}^*) = 1$，则松弛是紧的，可恢复原问题最优解。否则需**随机舍入**。

### 5.3 交替优化 / ADMM

将非凸QCQP分解为子问题交替求解（见 [[近端梯度法与ADMM]]）。

---

## 六、典型应用

### 6.1 最小二乘 + 正则化

$$\min \|\mathbf{A}\mathbf{x} - \mathbf{b}\|_2^2 \quad \text{s.t.} \quad \|\mathbf{x}\|_2^2 \leq R$$

（Trust-region子问题）

### 6.2 波束成形（信号处理）

$$\min \mathbf{w}^H \mathbf{R}_n \mathbf{w} \quad \text{s.t.} \quad \mathbf{w}^H \mathbf{R}_s \mathbf{w} \geq 1$$

### 6.3 传感器网络定位

$$\min \sum_{(i,j) \in \mathcal{E}} \left(\|\mathbf{x}_i - \mathbf{x}_j\|_2^2 - d_{ij}^2\right)^2$$

非凸QCQP，需SDR松弛求解。

### 6.4 支持向量机（SVM）训练

对偶SVM的形式是QP（特例）。

---

## 七、与其他问题类的关系

```
LP ⊂ QP ⊂ QCQP ⊂ SOCP ⊂ SDP
```

| 问题类 | 目标 | 不等式约束 | 等式约束 |
|--------|------|-----------|---------|
| LP | 线性 | 线性 | 线性 |
| QP | 二次 | 线性 | 线性 |
| QCQP | 二次 | 二次 | 线性 |
| SOCP | 线性 | 二阶锥 | 线性 |
| SDP | 线性 | LMI | 线性 |

---

## 相关笔记

- [[二次规划 QP]] — QCQP的特例
- [[二阶锥规划 SOCP]] — QCQP的锥推广
- [[半定规划 SDP]] — 非凸QCQP的SDP松弛
- [[凸优化问题标准形式]] — 建模规范
- [[对偶理论]] — QCQP的对偶性
