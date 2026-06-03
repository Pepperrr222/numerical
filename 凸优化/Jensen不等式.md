# Jensen不等式

Jensen不等式是[[凸函数]]最深刻的分析性质，它揭示了凸函数与期望运算之间的基本不等式关系。

[[凸函数]] | [[凸函数定义与基本性质]] | [[共轭函数]]

---

## 一、基本陈述

### 1.1 离散形式

若 $f$ 为凸函数，$\theta_i \geq 0$，$\sum_{i=1}^{k} \theta_i = 1$：

$$\boxed{f\left(\sum_{i=1}^{k} \theta_i \mathbf{x}_i\right) \leq \sum_{i=1}^{k} \theta_i f(\mathbf{x}_i)}$$

### 1.2 概率形式

若 $\mathbf{X}$ 是随机变量，取值于 $\text{dom }f$：

$$\boxed{f(\mathbb{E}[\mathbf{X}]) \leq \mathbb{E}[f(\mathbf{X})]}$$

### 1.3 一般测度形式

$$\boxed{f\left(\int_\Omega \mathbf{x} \, d\mu(\mathbf{x})\right) \leq \int_\Omega f(\mathbf{x}) \, d\mu(\mathbf{x})}$$

其中 $\mu$ 是 $\text{dom }f$ 上的概率测度。

---

## 二、证明思路

### 数学归纳法（离散形式）

对点数 $k$ 使用归纳法：
- $k=2$：由凸函数定义直接得到
- $k>2$：设 $\theta = \sum_{i=1}^{k-1} \theta_i$，则 $\mathbf{y} = \sum_{i=1}^{k-1} \frac{\theta_i}{\theta} \mathbf{x}_i$，然后：
  $$f(\theta \mathbf{y} + (1-\theta)\mathbf{x}_k) \leq \theta f(\mathbf{y}) + (1-\theta)f(\mathbf{x}_k)$$
  结合归纳假设 $f(\mathbf{y}) \leq \sum_{i=1}^{k-1} \frac{\theta_i}{\theta} f(\mathbf{x}_i)$ 即得。

### 支撑超平面证法（光滑情形）

若 $f$ 可微，由[[凸函数一阶条件|一阶条件]]：
$$f(\mathbf{x}) \geq f(\mathbb{E}[\mathbf{X}]) + \nabla f(\mathbb{E}[\mathbf{X}])^T (\mathbf{x} - \mathbb{E}[\mathbf{X}])$$

两边取期望：
$$\mathbb{E}[f(\mathbf{X})] \geq f(\mathbb{E}[\mathbf{X}]) + 0$$

---

## 三、经典应用

### 3.1 算术-几何平均不等式（AM-GM）

函数 $f(x) = -\log x$ 是 $\mathbb{R}_{++}$ 上的凸函数。

Jensen ⇒ $-\log\left(\frac{x_1+\cdots+x_n}{n}\right) \leq -\frac{\log x_1 + \cdots + \log x_n}{n}$

即：

$$\boxed{\frac{x_1 + \cdots + x_n}{n} \geq \sqrt[n]{x_1 \cdots x_n}}$$

### 3.2 Hölder不等式

由 $f(x) = e^x$ 的凸性推导而出。

### 3.3 Gibbs不等式 / KL散度非负

$f(x) = x\log x$（凸，$\mathbb{R}_{++}$ 上）

Jensen应用于概率分布 $p, q$：
$$\text{KL}(p \| q) = \sum p_i \log\frac{p_i}{q_i} \geq 0$$

### 3.4 Lyapunov不等式

对随机变量 $X$ 和 $0 < r < s$：
$$(\mathbb{E}[|X|^r])^{1/r} \leq (\mathbb{E}[|X|^s])^{1/s}$$

（由 $f(x) = x^{s/r}$（凸）的Jensen不等式得到）

### 3.5 Rao-Blackwell定理（统计）

条件期望减少了凸损失下的风险：
$$\mathbb{E}[L(\mathbb{E}[\hat{\theta} \mid T])] \leq \mathbb{E}[L(\hat{\theta})]$$

其中 $L$ 是凸损失函数。

---

## 四、Jensen不等式与信息论

### 4.1 熵的上界

离散分布 $p$ 的熵：
$$H(p) = -\sum p_i \log p_i \leq \log n$$

（$f(x) = -\log x$ 的凸性 + Jensen 对均匀分布的应用）

### 4.2 数据压缩

对任意编码方案，期望码长 $\geq H(p)$（熵是下界）。

---

## 五、Jensen不等式的逆

### 5.1 反向Jensen（对凹函数）

若 $f$ 为凹函数：$f(\mathbb{E}[X]) \geq \mathbb{E}[f(X)]$

### 5.2 Cramér-Rao型反向

对特定结构，可通过扰动参数的方向来界定 Jensen 不等式的紧度。

---

## 六、概率论中的Jensen不等式

### EM算法收敛性

EM算法的关键步骤使用Jensen不等式构造下界：

$$\log p(\mathbf{x} \mid \theta) = \log \sum_{\mathbf{z}} p(\mathbf{x}, \mathbf{z} \mid \theta) = \log \sum_{\mathbf{z}} q(\mathbf{z}) \frac{p(\mathbf{x}, \mathbf{z} \mid \theta)}{q(\mathbf{z})} \geq \sum_{\mathbf{z}} q(\mathbf{z}) \log \frac{p(\mathbf{x}, \mathbf{z} \mid \theta)}{q(\mathbf{z})}$$

（$\log$ 的凹性 + Jensen）

### 变分推断（ELBO）

$$\log p(\mathbf{x}) \geq \mathbb{E}_{q(\mathbf{z})}[\log p(\mathbf{x}, \mathbf{z})] - \mathbb{E}_{q(\mathbf{z})}[\log q(\mathbf{z})] = \text{ELBO}$$

---

## 相关笔记

- [[凸函数定义与基本性质]] — 凸函数的定义
- [[凸函数一阶条件]] — Jensen的梯度证明
- [[共轭函数]] — Fenchel不等式（Jensen的另一面）
- [[上镜图与下水平集]] — Jensen的几何解释
