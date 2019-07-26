---
title: 'GraphEmbedding(2): LINE'
tags:
  - graph embedding
categories:
  - graph embedding
---

## LINE[^1] 算法原理

LINE也是一种基于邻域相似假设的方法，只不过与DeepWalk使用DFS构造邻域不同的是，LINE可以看作是一种使用BFS构造邻域的算法。此外，LINE还可以应用在**带权图**中（DeepWalk仅能用于无权图）。

之前还提到不同的graph embedding方法的一个主要区别是对图中顶点之间的相似度的定义不同，所以先看一下LINE对于相似度的定义。

![ZTtOPA.png](https://s2.ax1x.com/2019/07/15/ZTtOPA.png)

### 一阶相似度

用于描述图中成对顶点之间的局部相似度，形式化描述为若  $u, v$ 之间存在直连边，则边权 $w_{u v}$ 即为两个顶点的相似度，若不存在直连边，则1阶相似度为0。 

参考 6,7

### 二阶相似度

$$
p_{u}=\left(w_{u, 1}, \dots, w_{u,|V|}\right)
$$ 表示顶点 $u$ 与所有其他顶点的一阶相似度
则 $u$ 与 $v$ 的二阶相似度可以通过 $p_u$ 和 $p_v$ 的相似度表示。

## 优化目标

### 一阶相似度

对于一条无向边 $(i, j)$ ，顶点 $v_i$ 和 $v_j$：

$\overrightarrow{u_{i}}$ 为顶点 $v_i$ 的低维向量表示

$v_i$ 和 $v_j$ 的联合概率为

$$
p_{1}\left(v_{i}, v_{j}\right)=\frac{1}{1+\exp \left(-\vec{u}_{i}^{T} \cdot \vec{u}_{j}\right)}
$$

定义经验分布，

$$
\hat{p_{1}}=\frac{w_{i j}}{W}, \quad W=\sum_{(i, j) \in E} w_{i j}
$$

**优化目标**为最小化两个分布之间的距离

$$
O_{1}=d\left(\hat{p}_{1}(\cdot, \cdot), p_{1}(\cdot, \cdot)\right)
$$

$d(\cdot, \cdot)$ 是两个分布之间的距离，常用 KL 散度表示。忽略常数项后有

$$
O_{1}=-\sum_{(i, j) \in E} w_{i j} \log p_{1}\left(v_{i}, v_{j}\right)
$$

这个公式很奇妙，先是两个节点联合概率**直白一点说就是匹配程度**取个 $\log$ ，然后在用连接强度作为权重。这里计算出来的只是一对节点，把所有的节点对都取完之后，也就算出来$u, v$ 的近似程度。

> 一阶相似度只能用于无向图当中

### 二阶相似度

每个顶点维护两个 embedding 向量，一个是该顶点本身的表示向量，另一个是该点作为其他顶点的上下文顶点时的表示向量。

对于有向边 $(i,j)$ ，定义给定顶点 $v_i$ 条件下，产生上下文（邻居）顶点 $v_j$ 的概率为

$$
p_{2}\left(v_{j} | v_{i}\right)=\frac{\exp \left(\vec{c}_{j}^{T} \cdot \vec{u}_{i}\right)}{\sum_{k=1}^{|V|} \exp \left(\vec{c}_{k}^{T} \cdot \vec{u}_{i}\right)}
$$

优化目标为 

$$
O_{2}=\sum_{i \in V} \lambda_{i} d\left(\hat{p}_{2}\left(\cdot | v_{i}\right), p_{2}\left(\cdot | v_{i}\right)\right)
$$

经验分布定义为

$$
\hat{p}_{2}\left(v_{j} | v_{i}\right)=\frac{w_{i j}}{d_{i}}
$$

$$
O_{2}=-\sum_{(i, j) \in E} w_{i j} \log p_{2}\left(v_{j} | v_{i}\right)
$$

## 优化技巧

### Negative sampling

目标函数变为

$$
\log \sigma\left(\vec{c}_{j}^{T} \cdot \vec{u}_{i}\right)+\sum_{i=1}^{K} E_{v_{n} \sim P_{n}(v)}\left[-\log \sigma\left(\vec{c}_{n}^{T} \cdot \vec{u}_{i}\right)\right]
$$

### Edge Sampling

Alias算法

### 低度数顶点问题

对于一些顶点由于其邻接点非常少会导致embedding向量的学习不充分，论文提到可以利用邻居的邻居构造样本进行学习，这里也暴露出LINE方法仅考虑一阶和二阶相似性，对高阶信息的利用不足。

### 新加入顶点


$$
-\sum_{j \in N(i)} w_{j i} \log p_{1}\left(v_{j}, v_{i}\right)_{\overrightarrow{\mathfrak{H}} \overrightarrow{\mathrm{k}}}-\sum_{j \in N(i)} w_{j i} \log p_{1}\left(v_{j} | v_{i}\right)
$$

## 总结

方法复杂，丑陋，可能只是一个启发式的模型

[^1]:https://arxiv.org/pdf/1503.03578.pdf
