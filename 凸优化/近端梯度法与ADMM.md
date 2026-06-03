# 近端梯度法与ADMM

当目标函数具有**复合结构** $f(\mathbf{x}) = g(\mathbf{x}) + h(\mathbf{x})$（$g$ 光滑凸，$h$ 非光滑凸但近端易算），近端梯度法比[[次梯度方法]]快得多。ADMM则利用增广Lagrange将约束耦合的问题分解为并行可解的子问题。

[[凸优化算法]] | [[次梯度方法]] | [[有约束凸优化算法]] | [[共轭函数]]

---

## 一、近端算子 (Proximal Operator)

### 1.1 定义

$$\text{prox}_{t h}(\mathbf{v}) = \arg\min_{\mathbf{x}} \left(h(\mathbf{x}) + \frac{1}{2t}\|\mathbf{x} - \mathbf{v}\|_2^2\right)$$

### 1.2 直观理解

近端算子 = 在 $\mathbf{v}$ 的**邻域内**寻找 $h$ 值小的 $\mathbf{x}$，参数 $t$ 控制领域大小。

- $t$ 小 → $\mathbf{x}$ 靠近 $\mathbf{v}$（近端效应强）
- $t$ 大 → $h$ 的影响更大

### 1.3 常见近端算子的闭式解

| $h(\mathbf{x})$ | $\text{prox}_{th}(\mathbf{v})$ | 名称 |
|----------------|-------------------------------|------|
| $0$ | $\mathbf{v}$ | 恒等 |
| $I_\mathcal{C}(\mathbf{x})$ | $P_\mathcal{C}(\mathbf{v})$ | Euclid投影 |
| $\|\mathbf{x}\|_1$ | $[\max(0, v_i - t)]\cdot\text{sign}(v_i)$ | 软阈值 (Soft-Thresholding) |
| $\|\mathbf{x}\|_2$ | $\max(0, 1 - t/\|\mathbf{v}\|_2)\cdot\mathbf{v}$ | 块软阈值 |
| $\|\mathbf{x}\|_*$（核范数） | $\mathbf{U}\text{diag}(\max(0,\sigma_i - t))\mathbf{V}^T$ | 奇异值阈值 |
| $\frac{1}{2}\|\mathbf{x}\|_2^2$ | $\frac{1}{1+t}\mathbf{v}$ | 缩放 |
| $-\sum \log x_i$ | $(v_i + \sqrt{v_i^2+4t})/2$ | （单纯形内）|

### 1.4 近端算子与Moreau包络

近端算子是Moreau包络的梯度：

$$M_{th}(\mathbf{v}) = \min_{\mathbf{x}} \left(h(\mathbf{x}) + \frac{1}{2t}\|\mathbf{x} - \mathbf{v}\|_2^2\right)$$

$$\nabla M_{th}(\mathbf{v}) = \frac{1}{t}(\mathbf{v} - \text{prox}_{th}(\mathbf{v}))$$

参见 [[共轭函数]]

---

## 二、近端梯度法 (Proximal Gradient Method / ISTA)

### 2.1 问题形式

$$\min_{\mathbf{x}} f(\mathbf{x}) = g(\mathbf{x}) + h(\mathbf{x})$$

- $g$：光滑凸，$\nabla g$ 为 $L$-Lipschitz连续
- $h$：凸（可能非光滑），近端算子有闭式解

### 2.2 ISTA迭代

$$\mathbf{x}^{(k+1)} = \text{prox}_{t_k h}\left(\mathbf{x}^{(k)} - t_k \nabla g(\mathbf{x}^{(k)})\right)$$

分解为两步：
1. **梯度步**（对光滑部分）：$\mathbf{y}^{(k)} = \mathbf{x}^{(k)} - t_k \nabla g(\mathbf{x}^{(k)})$
2. **近端步**（对非光滑部分）：$\mathbf{x}^{(k+1)} = \text{prox}_{t_k h}(\mathbf{y}^{(k)})$

> 梯度下降 + 近端投影 = 近端梯度法

### 2.3 收敛率

$$f(\mathbf{x}^{(k)}) - f^* \leq \frac{L\|\mathbf{x}^{(0)} - \mathbf{x}^*\|_2^2}{2k} = O(1/k)$$

**比次梯度法的 $O(1/\sqrt{k})$ 更快**。

### 2.4 FISTA（加速近端梯度法）

结合Nesterov动量：

$$\begin{aligned} \mathbf{y}^{(k)} &= \mathbf{x}^{(k)} + \frac{k-1}{k+2}(\mathbf{x}^{(k)} - \mathbf{x}^{(k-1)}) \\ \mathbf{x}^{(k+1)} &= \text{prox}_{t h}\left(\mathbf{y}^{(k)} - t \nabla g(\mathbf{y}^{(k)})\right) \end{aligned}$$

收敛率提升至 **$O(1/k^2)$**（对一阶方法最优）。

---

## 三、ADMM (Alternating Direction Method of Multipliers)

### 3.1 问题形式

$$\begin{aligned} \min_{\mathbf{x}, \mathbf{z}} \quad & f(\mathbf{x}) + g(\mathbf{z}) \\ \text{s.t.} \quad & \mathbf{A}\mathbf{x} + \mathbf{B}\mathbf{z} = \mathbf{c} \end{aligned}$$

- $f, g$ 为凸函数（可非光滑，但各自的近端算子易算）
- 变量通过**线性约束**耦合

### 3.2 增广Lagrange函数

$$L_\rho(\mathbf{x}, \mathbf{z}, \mathbf{y}) = f(\mathbf{x}) + g(\mathbf{z}) + \mathbf{y}^T (\mathbf{A}\mathbf{x} + \mathbf{B}\mathbf{z} - \mathbf{c}) + \frac{\rho}{2}\|\mathbf{A}\mathbf{x} + \mathbf{B}\mathbf{z} - \mathbf{c}\|_2^2$$

其中 $\rho > 0$ 是**增广参数**，$\mathbf{y}$ 是Lagrange乘子（对偶变量）。

### 3.3 ADMM迭代（三步交替）

$$\boxed{\begin{aligned} \mathbf{x}^{(k+1)} &= \arg\min_{\mathbf{x}} L_\rho(\mathbf{x}, \mathbf{z}^{(k)}, \mathbf{y}^{(k)}) \quad &\text{(x-子问题)} \\ \mathbf{z}^{(k+1)} &= \arg\min_{\mathbf{z}} L_\rho(\mathbf{x}^{(k+1)}, \mathbf{z}, \mathbf{y}^{(k)}) \quad &\text{(z-子问题)} \\ \mathbf{y}^{(k+1)} &= \mathbf{y}^{(k)} + \rho (\mathbf{A}\mathbf{x}^{(k+1)} + \mathbf{B}\mathbf{z}^{(k+1)} - \mathbf{c}) \quad &\text{(对偶更新)} \end{aligned}}$$

### 3.4 关键特性

- **分解性**：$\mathbf{x}$ 和 $\mathbf{z}$ 分别求解，可利用各自的特殊结构
- **对偶上升**：$\mathbf{y}$ 更新本质上是**对偶上升步**（步长 $\rho$）
- **收敛性**：对凸问题，在极温和条件下收敛（$f, g$ 不必光滑）
- 收敛速度一般（$O(1/k)$ 次线性），但实践中可接受

### 3.5 缩放形式的ADMM

定义缩放对偶变量 $\mathbf{u} = \mathbf{y}/\rho$，迭代等价于：

$$\begin{aligned} \mathbf{x}^{(k+1)} &= \arg\min_{\mathbf{x}} \left(f(\mathbf{x}) + \frac{\rho}{2}\|\mathbf{A}\mathbf{x} + \mathbf{B}\mathbf{z}^{(k)} - \mathbf{c} + \mathbf{u}^{(k)}\|_2^2\right) \\ \mathbf{z}^{(k+1)} &= \arg\min_{\mathbf{z}} \left(g(\mathbf{z}) + \frac{\rho}{2}\|\mathbf{A}\mathbf{x}^{(k+1)} + \mathbf{B}\mathbf{z} - \mathbf{c} + \mathbf{u}^{(k)}\|_2^2\right) \\ \mathbf{u}^{(k+1)} &= \mathbf{u}^{(k)} + (\mathbf{A}\mathbf{x}^{(k+1)} + \mathbf{B}\mathbf{z}^{(k+1)} - \mathbf{c}) \end{aligned}$$

---

## 四、ADMM的收敛性

### 4.1 假设

- $f$ 和 $g$ 为适当的闭凸函数
- 增广Lagrange函数存在鞍点

### 4.2 收敛结果

- **目标函数收敛**：$f(\mathbf{x}^{(k)}) + g(\mathbf{z}^{(k)}) \to p^*$
- **对偶变量收敛**：$\mathbf{y}^{(k)} \to \mathbf{y}^*$
- **约束违规收敛**：$\|\mathbf{A}\mathbf{x}^{(k)} + \mathbf{B}\mathbf{z}^{(k)} - \mathbf{c}\| \to 0$

收敛速度为 $O(1/k)$（次线性）。

### 4.3 加速条件

存在某些特殊结构（如强凸、光滑性）时，ADMM可达到**线性收敛**。

---

## 五、ADMM的经典应用

### 5.1 LASSO / 稀疏回归

$$\min_{\mathbf{x}} \frac{1}{2}\|\mathbf{A}\mathbf{x} - \mathbf{b}\|_2^2 + \lambda\|\mathbf{x}\|_1$$

ADMM形式：
- $f(\mathbf{x}) = \frac{1}{2}\|\mathbf{A}\mathbf{x} - \mathbf{b}\|_2^2$（光滑）
- $g(\mathbf{z}) = \lambda\|\mathbf{z}\|_1$（软阈值近端）
- 约束：$\mathbf{x} = \mathbf{z}$

### 5.2 鲁棒PCA

$$\min_{\mathbf{L}, \mathbf{S}} \|\mathbf{L}\|_* + \lambda\|\mathbf{S}\|_1 \quad \text{s.t.} \quad \mathbf{L} + \mathbf{S} = \mathbf{M}$$

- $f(\mathbf{L}) = \|\mathbf{L}\|_*$ ⇒ 奇异值阈值
- $g(\mathbf{S}) = \lambda\|\mathbf{S}\|_1$ ⇒ 软阈值

### 5.3 一致优化 (Consensus Optimization)

$$\min_{\mathbf{x}} \sum_{i=1}^{N} f_i(\mathbf{x})$$

引入局部变量 $\mathbf{x}_i$ 和全局变量 $\mathbf{z}$，约束 $\mathbf{x}_i = \mathbf{z}$。

ADMM并行化：每步各 $f_i$ 独立最小化，然后汇总平均。

---

## 六、$\rho$ 的选择

增广参数 $\rho$ 影响：
- $\rho$ 大 → 约束违反惩罚强，收敛快但子问题可能变硬
- $\rho$ 小 → 子问题简单但收敛慢

实践中常用**自适应$\rho$调整策略**：根据原始/对偶残差的相对大小动态调整。

---

## 相关笔记

- [[次梯度方法]] — 非光滑优化的另一种方法
- [[无约束凸优化算法]] — ISTA子问题的梯度步
- [[有约束凸优化算法]] — 增广Lagrange法与内点法
- [[共轭函数]] — Moreau包络与近端算子
- [[随机梯度下降 SGD]] — 大规模在线优化
