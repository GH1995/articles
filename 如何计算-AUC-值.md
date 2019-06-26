---
title: 如何计算 AUC 值
date: 2019-06-17 18:15:19
tags:
    - deep learning
---

三种方法：

# 1. 积分法

```python
auc = 0.0
height = 0.0

for each train exameple x_i y_i:
    if y_i == 1.0:
        height = height + 1/(tp+fn)
    else
        auc += height * 1/(tn + fp)

return auc
```

# 2. 曼-惠特尼法

Wilcoxon-Mann-Witney Test就是测试任意给一个正类样本和一个负类样本，正类样本的score有多大的概率大于负类样本的score

具体来说就是统计一下所有的 $M \times  N$($M$为正类样本的数目，$N$为负类样本的数目)个正负样本对中，有多少个组中的正样本的score大于负样本的score。当二元组中正负样本的 score相等的时候，按照0.5计算。然后除以$M \times N$。实现这个方法的复杂度为$O(n^2)$。$n$为样本数（即$n=M+N$）

$$
auc = \frac{\sum pos\_{score} > neg\_{score} + 0.5 \times \sum (pos\_{score} = neg\_{score})}{ M \times N }
$$

# 3. 曼-惠特尼法加强[^1]

1. 首先对 score 从大到小排序，然后令最大 score 对应的 sample 的rank为$n$，第二大score对应sample的rank为$n-1$，以此类推
2. 然后把所有的正类样本的rank相加**交错相加**
3. 再减去$\frac{M(M+1)}{2}$

得到的就是所有的样本中**有多少对正类样本的score大于负类样本的score**，然后再除以$M\times N$。

[^1]:参考[这里](https://blog.csdn.net/qq_22238533/article/details/78666436)
