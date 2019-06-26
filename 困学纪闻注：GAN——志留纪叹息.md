---
title: 困学纪闻注：GAN——志留纪叹息
date: 2019-06-20 10:45:16
tags:
    - deep learning
    - todo
---

## 显式密度模型和隐式密度模型

## 网络分解

![生成对抗网络的流程图](https://s2.ax1x.com/2019/06/20/VjrZFO.png)

### 判别网络

$$
p(y=1 | \mathbf{x})=D(\mathbf{x}, \phi)
$$

判别网络的 目标函数为最小化交叉熵，即最大化对数似然。


### 生成网络


$$
\max _{\theta}\left(\mathbb{E}_{\mathbf{z} \sim p(\mathbf{z})}[\log D(G(\mathbf{z}, \theta), \phi)]\right)
$$

为什么不使用$(1-D(G(\mathbf{z}, \theta), \phi)) \rightarrow 1$？
$\log$在$x$接近1时的梯度要比接近0时的梯度小很多，接近“饱和”区间。这样，当判别网络$D$以很高的概率认为生成网络$G$产生的样本是“假”样本。

## 训练

每次迭代时，判别网络更新$K$ 次而生成网络更新一次，即首先要保证判别网络足够强才能开始训练生成网络。

![训练](https://s2.ax1x.com/2019/06/20/VjcF3D.png)


## 一个生成对抗网络的具体实现：DCGAN

## 模型分析

最小化最大化游戏

$$
\begin{array}{c}{\min _{\theta} \max _{\phi}\left(\mathbb{E}_{\mathbf{x} \sim p_{r}(\mathbf{x})}[\log D(\mathbf{x}, \phi)]+\mathbb{E}_{\mathbf{x} \sim p_{\theta}(\mathbf{x})}[\log (1-D(\mathbf{x}, \phi))]\right)} \\ {=\min _{\theta} \max _{\phi}\left(\mathbb{E}_{\mathbf{x} \sim p_{r}(\mathbf{x})}[\log D(\mathbf{x}, \phi)]+\mathbb{E}_{\mathbf{z} \sim p(\mathbf{z})}[\log (1-D(G(\mathbf{z}, \theta), \phi))]\right)}\end{array}
$$

最优的判别器

$$
D^{\star}(\mathbf{x})=\frac{p_{r}(\mathbf{x})}{p_{r}(\mathbf{x})+p_{\theta}(\mathbf{x})}
$$

将最优的判别器$D^{\star}(\mathbf{x})$ 代入公式

$$
\mathcal{L}\left(G | D^{*}\right)=2 D_{\mathrm{JS}}\left(p_{r} \| p_{\theta}\right)-2 \log 2
$$

当判断网络为最优时，生成网络的优化目标是最小化真实分布$p_{r}$和模型分布$p_{\theta}$ 之间的JS散度。当两个分布相同时，JS散度为0，最优生成网络$G^{\star}$对应的损失为$L\left(G^{\star} | D^{\star}\right)=-2 \log 2$。

然而，JS散度的一个问题是：当两个分布没有重叠时，它们之间的JS散度恒等于常数$\log 2$。对生成网络来说，目标函数关于参数的梯度为0。

### 模型坍塌

## 改进模型

### W-GAN

略
