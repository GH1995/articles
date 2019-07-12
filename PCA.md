---
title: 困学纪闻注：PCA
date: 2019-03-18 13:02:52
tags:
    - deep learning
    - machine learning
    - todo
---

PCA(Principal Component Analysis, PCA) 数据降维方法，转换后数据的**方差最大**。

选择数据方差最大的方向进行投影，才能最大化数据的差异性，保留更多的原始数据信息。

一组 $d$ 维样本$\mathbf{x} \in \mathbb{R}^d, 1 \leq n \leq N$，将其投影到一维空间中，投影向量为 $\mathbf{w} \in \mathbb{R}^d$，不是一般性，限制 $\mathbf{w}$ 的模为 $1$ ，即 $\mathbb{w}^T \mathbf{w} = 1$.

## 计算每个样本点的投影表示 $z^(n)$

每个样本点 $\mathbf{x}^{(n)}$ 投影后 $1 \times d \times d \times 1$
$$
z^{(n)}=\mathbf{w}^{\mathrm{T}} \mathbf{x}^{(n)}
$$

##计算所有样本投影后的方差

$$
\begin{aligned} \sigma(X ; \mathbf{w}) &=\frac{1}{N} \sum_{n=1}^{N}\left(\mathbf{w}^{\mathrm{T}} \mathbf{x}^{(n)}-\mathbf{w}^{\mathrm{T}} \overline{\mathbf{x}}\right)^{2} \\ &=\frac{1}{N}\left(\mathbf{w}^{\mathrm{T}} X-\mathbf{w}^{\mathrm{T}} \overline{X}\right)\left(\mathbf{w}^{\mathrm{T}} X-\mathbf{w}^{\mathrm{T}} \overline{X}\right)^{\mathrm{T}} \\ &=\mathbf{w}^{\mathrm{T}} S \mathbf{w} \end{aligned}
$$

$S=\frac{1}{N}(X-\overline{X})(X-\overline{X})^{\mathrm{T}}$ 是原始样本的协方差矩阵。

## 拉格朗日法求最大投影方差

$$
\max _{\mathbf{w}} \mathbf{w}^{\mathrm{T}} S \mathbf{w}+\lambda\left(1-\mathbf{w}^{\mathrm{T}} \mathbf{w}\right)
$$

对上式求导并令导数等于0，得到

$$
S \mathbf{w}=\lambda \mathbf{w}
$$

只需要将$S$的特征值从大到小排列，保留前$d^{\prime}$个特征向量，其对应的特征向量即使最优的投影矩阵
