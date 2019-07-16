---
title: Python import 小结
tags:
  - Python
categories:
  - python
date: 2019-06-19 10:38:02
---

Python 的 `import` 很简单，记住关键一句话就行。

> **关键是能够在`sys.path`里面找到通向模块文件的路径**

分三种情况：

1. 主程序与模块在同一目录下：
这时候Python能自己找到自己的兄弟
```python
import mod1
```
2. 主程序是在上一层
Python 不会把所有的子文件夹都加入路径的，别想了！
这时候需要在子文件夹中加入`__init__.py`指明这是一个模块，然后
```python
import mod2.mod2 
```
3. 主程序导入上层目录中模块或其他目录(平级)下的模块
```python
import sys
sys.path.append("..")

import mod1 # 上一层
import mod2.mod2    # 堂兄弟文件夹
```

# 总结

关键还是找路径
