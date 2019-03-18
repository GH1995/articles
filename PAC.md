---
title: PAC
date: 2019-03-18 13:02:52
tags:
    - 无监督学习
---

PAC(Principal Component Analysis, PAC) 数据降维方法，转换后数据的**方差最大**。

选择数据方差最大的方向进行投影，才能最大化数据的差异性，保留更多的原始数据信息。

一组 $d$ 维样本$\mathbf{x} \in \mathbb{R}^d, 1 \leq n \leq N$，将其投影到一维空间中，投影向量为 $\mathbf{w} \in \mathbb{R}^d$，不是一般性，限制 $\mathbf{w}$ 的模为 $1$ ，即 $\mathbb{w}^T \mathbf{w} = 1$.

每个样本点 $\mathbf{x}^{(n)}$ 投影后 $1 \times d \times d \times 1$
$$
z^{(n)}  = \mathbf{w}^T \mathbf{(n)}
$$
