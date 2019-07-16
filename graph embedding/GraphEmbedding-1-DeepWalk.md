---
title: 'GraphEmbedding(1): DeepWalk'
tags:
  - graph embedding
categories:
  - graph embedding
---

## 图表示学习

- 数据结构
    - 最小生成树(Prim, Kruskal)
    - 最短路径(Dijkstra, Floyed)
    - 其他：拓扑排序，关键路径
- 概率图模型
    - 表示
    - 推断
    - 学习
- 图神经网络
    - GraphEmbedding(基于随机游走)
    - Graph CNN（基于邻居汇聚）

Graph Embedding技术将图中的节点以低维稠密向量的形式进行表达，要求在原始图中相似(不同的方法对相似的定义不同)的节点其在低维表达空间也接近。得到的表达向量可以用来进行下游任务，如节点分类，链接预测，可视化或重构原始图等。 

## DeepWalk 原理

KDD 2014[^1]

DeepWalk的思想类似word2vec，使用**图中节点与节点的共现关系**来学习节点的向量表示。那么关键的问题就是如何来描述节点与节点的共现关系，DeepWalk给出的方法是使用随机游走(RandomWalk)的方式在图中进行节点采样。

RandomWalk是一种**可重复访问已访问节点**的深度优先遍历算法。给定当前访问起始节点，从其邻居中随机采样节点作为下一个访问节点，重复此过程，直到访问序列长度满足预设条件。 

获取足够数量的节点访问序列后，使用skip-gram model 进行向量学习。

## 结语

In our view, language modeline  is actually sampling from an unobservable language graph. We believe that insights obtained from modeling observable graphs may in turn yield improvements to modeling unobservable ones.  

![  ](https://s2.ax1x.com/2019/07/15/ZT83TA.png)

[^1]:http://www.perozzi.net/publications/14_kdd_deepwalk.pdf


