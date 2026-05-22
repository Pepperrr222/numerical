# 随机梯度下降 (Stochastic Gradient Descent, SGD)

SGD是处理**大规模**和**在线**凸优化的核心方法。它用随机采样的梯度代替全梯度，每次迭代仅需 $O(1)$ 样本的计算量。

[[凸优化算法]] | [[无约束凸优化算法]] | [[次梯度方法]]

---

## 一、问题形式

### 1.1 有限和结构

$$f(\mathbf{x}) = \frac{1}{N}\sum_{i=1}^{N} f_i(\mathbf{x})$$

（如经验风险最小化，$N$ 为样本量）

### 1.2 期望形式

$$f(\mathbf{x}) = \mathbb{E}_{\xi}[f(\mathbf{x}; \xi)]$$

（如期望风险最小化，$\xi$ 为随机样本）

---

## 二、基本SGD算法

### 2.1 迭代

$$\mathbf{x}^{(k+1)} = \mathbf{x}^{(k)} - t_k \nabla f_{i_k}(\mathbf{x}^{(k)})$$

其中 $i_k$ 从 $\{1,\ldots,N\}$ 中随机采样。

### 2.2 无偏性

$$\mathbb{E}_{i_k}[\nabla f_{i_k}(\mathbf{x})] = \nabla f(\mathbf{x})$$

即随机梯度是全梯度的**无偏估计**。

### 2.3 方差

SGD的收敛受随机梯度**方差**限制：

$$\mathbb{E}\|\nabla f_i(\mathbf{x}) - \nabla f(\mathbf{x})\|_2^2 \leq \sigma^2$$

大方差 ⇒ 慢收敛（即使 $f$ 强凸，SGD也只能达到次线性收敛）。

---

## 三、步长策略

### 3.1 经典衰减步长

$$t_k = \frac{t_0}{1 + k/N} \quad \text{或} \quad t_k = \frac{t_0}{\sqrt{k}}$$

保证 $\sum t_k = \infty$ 和 $\sum t_k^2 < \infty$。

### 3.2 恒定步长

$$t_k \equiv t$$

收敛到最优解附近的一个**邻域**（大小为 $O(t\sigma^2)$），不收敛到精确最优值。

### 3.3 步长对收敛的影响

- 大步长 → 快速初始下降，但最终震荡大
- 小步长 → 低速下降，但最终更接近最优解

---

## 四、SGD的收敛性

### 4.1 光滑凸情况

对 $L$-光滑凸函数，使用衰减步长 $t_k = \frac{1}{L\sqrt{k}}$：

$$\mathbb{E}[f(\bar{\mathbf{x}}^{(K)})] - f^* \leq \frac{L\|\mathbf{x}^{(0)} - \mathbf{x}^*\|_2^2}{2K} + \frac{\sigma^2}{L\sqrt{K}}$$

其中 $\bar{\mathbf{x}}^{(K)} = \frac{1}{K}\sum_{k=1}^{K} \mathbf{x}^{(k)}$ 为Polyak平均。

收敛率 $O(1/\sqrt{K})$。

### 4.2 强凸情况

对 $\mu$-强凸 + $L$-光滑函数：

$$\mathbb{E}\|\mathbf{x}^{(k)} - \mathbf{x}^*\|^2 \leq O\left(\frac{1}{k}\right)$$

（次线性收敛，即使强凸。对比全梯度下降的线性收敛）

### 4.3 SGD的固有局限

即使在强凸条件下，SGD也只能达到**次线性收敛**，这是因为随机梯度的**非零方差**效应在最优解附近不消失。

**方案**：方差缩减技术（SVRG、SAGA等）可恢复线性收敛。

---

## 五、Mini-Batch SGD

### 5.1 迭代

$$\mathbf{x}^{(k+1)} = \mathbf{x}^{(k)} - t_k \cdot \frac{1}{|B_k|}\sum_{i \in B_k} \nabla f_i(\mathbf{x}^{(k)})$$

其中 $B_k$ 是从 $\{1,\ldots,N\}$ 随机采样的小批量（mini-batch）。

### 5.2 方差缩减

$$\text{Var}\left(\frac{1}{|B|}\sum_{i \in B} \nabla f_i\right) = \frac{\sigma^2}{|B|}$$

批大小增大 → 方差减小（但计算量线性增加）。

### 5.3 实际权衡

- 小批量（$|B|=32 \sim 256$）：计算效率高（GPU/TPU并行）
- 大批量（$|B| \to N$）：方差小但每步代价大

---

## 六、方差缩减方法

### 6.1 SVRG (Stochastic Variance Reduced Gradient)

周期性计算全梯度，以此修正随机梯度：

$$\mathbf{g}^{(k)} = \nabla f_{i_k}(\mathbf{x}^{(k)}) - \nabla f_{i_k}(\tilde{\mathbf{x}}) + \nabla f(\tilde{\mathbf{x}})$$

其中 $\tilde{\mathbf{x}}$ 是上一次全梯度快照点。

**收敛率**：强凸情况下线性收敛 $O((1 - \mu/L)^k)$。

### 6.2 SAGA

维护过去每个样本梯度的表，与SVRG类似但不需要周期全梯度。

### 6.3 SAG (Stochastic Average Gradient)

存储每个样本的最近梯度，用全部存储值的平均更新。

---

## 七、自适应方法

### 7.1 AdaGrad

每维独立学习率，根据历史梯度平方累加自适应缩放：

$$x_i^{(k+1)} = x_i^{(k)} - \frac{\eta}{\sqrt{G_{ii}^{(k)} + \epsilon}} \cdot g_i^{(k)}$$

其中 $G_{ii}^{(k)} = \sum_{j=1}^{k} (g_i^{(j)})^2$。

**优点**：适合稀疏特征（低频特征自动放大步长）
**缺点**：学习率单调衰减，可能过早停滞

### 7.2 RMSProp

用指数移动平均代替累加和：

$$G_{ii}^{(k)} = \beta G_{ii}^{(k-1)} + (1-\beta)(g_i^{(k)})^2$$

避免了AdaGrad的单调衰减问题。

### 7.3 Adam (Adaptive Moment Estimation)

结合**动量**（一阶矩）和**自适应学习率**（二阶矩）：

$$\begin{aligned} \mathbf{m}^{(k)} &= \beta_1 \mathbf{m}^{(k-1)} + (1-\beta_1) \mathbf{g}^{(k)} \\ \mathbf{v}^{(k)} &= \beta_2 \mathbf{v}^{(k-1)} + (1-\beta_2) (\mathbf{g}^{(k)})^2 \\ \hat{\mathbf{m}}^{(k)} &= \frac{\mathbf{m}^{(k)}}{1 - \beta_1^k}, \quad \hat{\mathbf{v}}^{(k)} = \frac{\mathbf{v}^{(k)}}{1 - \beta_2^k} \\ \mathbf{x}^{(k+1)} &= \mathbf{x}^{(k)} - t \cdot \frac{\hat{\mathbf{m}}^{(k)}}{\sqrt{\hat{\mathbf{v}}^{(k)}} + \epsilon} \end{aligned}$$

**Adam是当前深度学习/大规模优化中最广泛使用的优化器**。

---

## 八、学习率调度 (Learning Rate Scheduling)

- **步进衰减**：每 $M$ 步降低学习率为原来的 $\gamma$ 倍
- **余弦退火**：$t_k = t_{\max} \cdot \frac{1}{2}(1 + \cos(\pi k/K))$
- **热身**：前 $W$ 步线性增大学习率，之后衰减
- **循环学习率**：周期性升降，逃逸局部凹槽

---

## 九、全部算法速查表

| 方法 | 迭代复杂度 | 存储 | 适用场景 |
|------|-----------|------|---------|
| SGD | $O(1/\sqrt{K})$ | $O(n)$ | 大规模在线学习 |
| Mini-Batch SGD | $O(1/\sqrt{K})$ | $O(n)$ | GPU并行 |
| SVRG | $O((1-\mu/L)^K)$ | $O(n)$ | 精解 |
| Adam | 在实践中极快 | $O(3n)$ | 深度学习默认 |
| AdaGrad | $O(1/\sqrt{K})$ | $O(n)$ | 稀疏特征 |
| 全梯度下降 | $O(1/K)$ | $O(n)$ | 中等规模 |
| Nesterov | $O(1/K^2)$ | $O(n)$ | 中等规模 |

---

## 相关笔记

- [[无约束凸优化算法]] — 全梯度方法
- [[次梯度方法]] — 非光滑版的SGD
- [[凸优化算法]] — 算法总览
- [[近端梯度法与ADMM]] — 正则化问题与分布式
