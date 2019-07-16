---
title: 困学纪闻注：Logistic 回归
tags:
  - machine learning
  - todo
categories:
  - 深度学习
date: 2019-06-19 18:58:24
---

## 模型
$$
\begin{aligned} p(y=1 | \mathbf{x}) &=\sigma\left(\mathbf{w}^{\mathrm{T}} \mathbf{x}\right) \\ & \triangleq \frac{1}{1+\exp \left(-\mathbf{w}^{\mathrm{T}} \mathbf{x}\right)} \end{aligned}
$$

$$
\begin{aligned} p(y=0 | \mathbf{x}) &=1-p(y=1 | \mathbf{x}) \\ &=\frac{\exp \left(-\mathbf{w}^{\mathrm{T}} \mathbf{x}\right)}{1+\exp \left(-\mathbf{w}^{\mathrm{T}} \mathbf{x}\right)} \end{aligned}
$$

## 参数学习

$$
\hat{y}^{(n)}=\sigma\left(\mathbf{w}^{\mathrm{T}} \mathbf{x}^{(n)}\right), \qquad 1 \leq n \leq N
$$

风险函数
$$
\mathcal{R}(\mathbf{w})=-\frac{1}{N} \sum_{n=1}^{N}\left(y^{(n)} \log \hat{y}^{(n)}+\left(1-y^{(n)}\right) \log \left(1-\hat{y}^{(n)}\right)\right)
$$

$\hat{y}$为 Logistic 函数，故有
$$
\frac{\partial \hat{y}}{\partial \mathbf{w}}=\hat{y}^{(n)}\left(1-\hat{y}^{(n)}\right)
$$

$$
\begin{aligned} \frac{\partial \mathcal{R}(\mathbf{w})}{\partial \mathbf{w}} &=-\frac{1}{N} \sum_{n=1}^{N}\left(y^{(n)} \frac{\hat{y}^{(n)}\left(1-\hat{y}^{(n)}\right)}{\hat{y}^{(n)}} \mathbf{x}^{(n)}-\left(1-y^{(n)}\right) \frac{\hat{y}^{(n)}\left(1-\hat{y}^{(n)}\right)}{1-\hat{y}^{(n)}} \mathbf{x}^{(n)}\right) \\ &=-\frac{1}{N} \sum_{n=1}^{N}\left(y^{(n)}\left(1-\hat{y}^{(n)}\right) \mathbf{x}^{(n)}-\left(1-y^{(n)}\right) \hat{y}^{(n)}
\mathbf{x}^{(n)}\right) \\ &=-\frac{1}{N} \sum_{n=1}^{N} \mathbf{x}^{(n)}\left(y^{(n)}-\hat{y}^{(n)}\right) \end{aligned}
$$


$$
\mathbf{w}_{t+1} \leftarrow \mathbf{w}_{t}+\alpha \frac{1}{N} \sum_{n=1}^{N} \mathbf{x}^{(n)}\left(y^{(n)}-\hat{y}_{\mathbf{w}_{t}}^{(n)}\right)
$$
