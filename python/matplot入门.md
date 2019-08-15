---
title: matplot入门
categories:
  - python
date: 2019-08-05 21:43:04
tags:
---




```python
import matplotlib.pyplot as plt
import numpy as np
```

## 画一个简单图形


```python
x = np.linspace(0, 2 * np.pi, 50)
y = np.sin(x)
```


```python
plt.plot(x, np.sin(x))
plt.show()
```

![eRtRwF.png](https://s2.ax1x.com/2019/08/05/eRtRwF.png)


## 在一张图上绘制两个数据集


```python
plt.plot(x, np.sin(x), x, np.sin(2 * x))
plt.show()
```


![eRt2eU.png](https://s2.ax1x.com/2019/08/05/eRt2eU.png)

## 自定义图形的外观



```python
plt.plot(x, np.sin(x), 'r-o', x, np.cos(x), 'g--')
plt.show()
```


![eRt5WR.png](https://s2.ax1x.com/2019/08/05/eRt5WR.png)

## 使用子图

使用子图只需要一个额外的步骤，就可以像前面的例子一样绘制数据集。即在调用 plot() 函数之前需要先调用 subplot() 函数。该函数的第一个参数代表子图的总行数，第二个参数代表子图的总列数，第三个参数代表活跃区域。

活跃区域代表当前子图所在绘图区域，绘图区域是按从左至右，从上至下的顺序编号。例如在 4×4 的方格上，活跃区域 6 在方格上的坐标为 (2, 2)。


```python
plt.subplot(2, 1, 1) # 聚焦活跃区域
plt.plot(x, np.sin(x), 'r')  # 绘制
plt.subplot(2, 1, 2)
plt.plot(x, np.cos(x), 'g')
plt.show()
```


![eRtoS1.png](https://s2.ax1x.com/2019/08/05/eRtoS1.png)

## 简单的散点图


调用 scatter() 函数并传入两个分别代表 x 坐标和 y 坐标的数组。注意，我们通过 plot 命令并将线的样式设置为 'bo' 也可以实现同样的效果。


```python
plt.scatter(x, y)
plt.show()
```

![eRtTQx.png](https://s2.ax1x.com/2019/08/05/eRtTQx.png)


## 彩色映射散点图



```python
x = np.random.rand(1000)
y = np.random.rand(1000)
size = np.random.rand(1000) * 50
color = np.random.rand(1000)
```


```python
plt.scatter(x, y, size, color)
plt.colorbar()
plt.show()
```


![eRt4Y9.png](https://s2.ax1x.com/2019/08/05/eRt4Y9.png)

## 直方图



```python
x = np.random.randn(1000)
plt.hist(x, 50)
plt.show()
```


![eRthFJ.png](https://s2.ax1x.com/2019/08/05/eRthFJ.png)

## 标题，标签和图例



```python
x = np.linspace(0, 2*np.pi, 50)
```


```python
plt.plot(x, np.sin(x), 'r-x', label='sin(x)')
plt.plot(x, np.cos(x), 'g-^', label='cos(x)')
plt.legend() # 展示图例
plt.xlabel('Rads')
plt.ylabel('Amplitude')
plt.title('sin and cos')
plt.show()
```


![eRtWo4.png](https://s2.ax1x.com/2019/08/05/eRtWo4.png)

