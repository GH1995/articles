---
title: 神奇动物在哪里？——softmax 函数的实现
date: 2019-06-22 00:03:21
tags:
    - deep learning
---

## 背景[^1]

**溢出问题**：softmax 函数在实现中要进行指数运算，指数函数值容易变得非常大，从而导致溢出。

## 解决办法

在进行 softmax 的指数函数的运算时，加上（或者减去） 某个常数并不会改变运算的结果。这里的 $C$ 可以使用任何值，但是为了防 止溢出，一般会使用输入信号中的最大值。

$$
\begin{aligned} y_{k}=\frac{\exp \left(a_{k}\right)}{\sum_{i=1}^{n} \exp \left(a_{i}\right)} &=\frac{\operatorname{Cexp}\left(a_{k}\right)}{\mathrm{C} \sum_{i=1}^{n} \exp \left(a_{i}\right)} \\ &=\frac{\exp \left(a_{k}+\log \mathrm{C}\right)}{\sum_{i=1}^{n} \exp \left(a_{i}+\log \mathrm{C}\right)} \\ &=\frac{\exp \left(a_{k}+\mathrm{C}^{\prime}\right)}{\sum_{i=1}^{n} \exp \left(a_{i}+\mathrm{C}^{\prime}\right)} \end{aligned}
$$

## 样例

```python
a = np.array([1010, 1000, 900])
```


```python
np.exp(a)/np.sum(np.exp(a))
```

    C:\Users\GuanHua\Anaconda3\lib\site-packages\ipykernel_launcher.py:1: RuntimeWarning: invalid value encountered in true_divide
      """Entry point for launching an IPython kernel.

    array([nan, nan, nan])

```python
c = np.max(a)
```


```python
a - c
```




    array([   0,  -10, -110])




```python
np.exp(a-c)/np.sum(np.exp(a-c))
```




    array([9.99954602e-01, 4.53978687e-05, 1.68883521e-48])



## 实现

```python
def softmax(a):
    c = np.max(a)
    exp_a = np.exp(a - c)
    sum_exp_a = np.sum(exp_a)
    
    y = exp_a/sum_exp_a
    
    return y
```

[^1]: 参考[这里](https://book.douban.com/subject/30270959/)第四章，第66页
