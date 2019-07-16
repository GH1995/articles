---
title: 建炎以来系年要录：EM 算法
tags:
  - machine learning
  - todo
categories:
  - 统计学习方法
date: 2019-06-21 00:35:09
---

EM 算法是一种迭代算法，用于含有隐变量的概率模型参数的极大似然估计。每次迭代由两步组成：
- $E$ 步，求期望 (expectation)
- $M$ 步，求极大值 (maximization)，直至收敛为止。

## 算法

1. $E$步：$\theta(i)$ 为 $i$ 次迭代参数 $\theta$ 的估计值，在第 $i+1$ 次迭代的 $E$ 步，计算
$$
\begin{aligned} Q\left(\theta, \theta^{(i)}\right) &=E_{Z}\left[\log P(Y, Z | \theta) | Y, \theta^{(i)}\right] \\ &=\sum_{Z} \log P(Y, Z | \theta) P\left(Z | Y, \theta^{(i)}\right) \end{aligned}
$$
$P(Z|Y，\theta(i))$ 是在给定观测数据 $Y$ 和当前参数估计 $\theta(i)$ 下隐变量数据Z的条件概率分布。
2. $M$ 步：求使 $Q(\theta, \theta(i))$ 极大化的 $\theta$，确定第 $i+1$ 次迭代的参数的估计值
$$
\theta^{(i+1)}=\arg \max _{\theta} Q\left(\theta, \theta^{(i)}\right)
$$

EM 算法是通过不断求解下界的极大化逼近求解对数似然函数极大化的算法。可以用于生成模型的非监督学习。生成模型由联合概率分布 $P(X，Y)$ 表示。
