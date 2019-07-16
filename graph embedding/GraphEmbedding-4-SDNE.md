---
title: 'GraphEmbedding(4): SDNE'
tags:
  - graph embedding
categories:
  - graph embedding
---

## SNDE 算法原理

SNDE 可以看作是基于LINE的扩展，同时也是第一个将深度学习应用于网络表示学习中的方法。 不清楚LINE的同学可以参考

SDNE使用一个自动编码器结构来同时优化1阶和2阶相似度(LINE是分别优化的)，学习得到的向量表示能够保留局部和全局结构，并且对稀疏网络具有鲁棒性。**这个想法不错**

### 相似度定义

SDNE中的相似度定义和LINE是一样的。

- 一阶相似度衡量的是相邻的两个顶点对之间相似性
- 二阶相似度衡量的是，两个顶点他们的邻居集合的相似程度。

### 二阶相似度优化目标

$$
L_{2 n d}=\sum_{i=1}^{n}\left\|\hat{x}_{i}-x_{i}\right\|_{2}^{2}
$$

输入：邻接矩阵

对于第 $i$ 个顶点，有 $x_i = s_i$，每个 $s_i$ 都包含了顶点 $i$ 的邻居结构信息。

$$
L_{2 n d}=\sum_{i=1}^{n}\left\|\left(\hat{x}_{i}-x_{i}\right) \odot \mathbf{b}_{\mathbf{i}}\right\|_{2}^{2}=\|(\hat{X}-X) \odot B\|_{F}^{2}
$$

$\mathbf{b}_{\mathbf{i}}=\left\{b_{i, j}\right\}_{j=1}^{n}$，若 $s_{i,j} = 0$，则 $b_{i,j} = 1$ ，否则 $b_{i, j}=\beta>1$

## 一阶相似度优化目标

$$
L_{1 s t}=\sum_{i, j=1}^{n} s_{i, j}\left\|\mathbf{y}_{\mathbf{i}}^{(K)}-\mathbf{y}_{\mathbf{j}}^{(K)}\right\|_{2}^{2}=\sum_{i, j=1}^{n} s_{i, j}\left\|\mathbf{y}_{\mathbf{i}}-\mathbf{y}_{\mathbf{j}}\right\|_{2}^{2}
$$

$$
L_{1 s t}=\sum_{i, j=1}^{n}\left\|\mathbf{y}_{i}-\mathbf{y}_{j}\right\|_{2}^{2}=2 \operatorname{tr}\left(Y^{T} L Y\right)
$$

## 整体优化目标

$$
L_{m i x}=L_{2 n d}+\alpha L_{1 s t}+\nu L_{r e g}
$$



