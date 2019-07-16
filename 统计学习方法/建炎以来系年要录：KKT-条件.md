---
title: 建炎以来系年要录：KKT 条件
tags:
  - machine learning
categories:
  - 统计学习方法
date: 2019-06-20 11:28:24
---

## 有不等式约束的优化问题

把不等式约束，等式约束和优化问题合并为一个式子。假设有多个等式约束 $h(x)$ 和不等式约束 $g(x)$

$$
L(\boldsymbol{x}, \boldsymbol{\lambda}, \boldsymbol{\mu})=f(\boldsymbol{x})+\sum_{i=1}^{m} \lambda_{i} h_{i}(\boldsymbol{x})+\sum_{i=1}^{n} \mu_{j} g_{j}(\boldsymbol{x})
$$


则不等式约束引入的KKT条件如下

$$
\left\{\begin{array}{l}{g_{j}(\boldsymbol{x}) \leqslant 0} \\ {\mu_{j} \geqslant 0} \\ {\mu_{j} g_{j}(\boldsymbol{x})=0}\end{array}\right.
$$

当 $g(x)=0$ 时，那么只需要使 $L$ 对 $x$ 求导为零，使 $h(x)$ 为零，使 $\mu g(x)$ 为零三式即可求解候选最优值。
