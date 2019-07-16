---
title: 'GraphEmbedding(3): node2vec'
tags:
  - graph embedding
  - todo
categories:
  - graph embedding
---

node2vec和deepwalk非常类似，主要区别在于顶点序列的采样策略不同

## node2vec 算法原理

### 优化目标

- $f(u)$ 是将顶点 $u$ 映射为 embedding 向量的映射函数
- $N_S(u)$ 是通过采样策略 $S$ 采样出的顶点 $u$ 的近邻顶点集合

优化目标是给定每个顶点条件下，令其近邻顶点（如何定义近邻顶点很重要）出现的概率最大。

$$
\max _{f} \sum_{u \in V} \log \operatorname{Pr}\left(N_{S}(U) | f(u)\right)
$$

提出了两个假设

1. 条件独立性假设
假设给定源顶点下，其近邻顶点出现的概率与近邻集合中其余顶点无关。
2. 特征空间对策假设
这里是说一个顶点作为源顶点和作为近邻顶点的时候共享同一套embedding向量。

## 顶点序列采样策略

## 学习算法

Alias 

