---
title: 给忙碌烦躁者的基本魔咒：__init__.py 文件解析
date: 2019-06-27 11:12:58
tags:
    - Python
    - 常识
---

{% cq %} 
剃头咒……可是龙没有头发……胡椒粉咒……那大概只会增强龙的火力……硬舌咒……那正是他需要的，给火龙再加一个武器……
{% endcq %}

## 在 `__init__.py` 文件中批量导入我们所需要的模块

特殊文件`__init__.py`可以为空，也可以包含属于包的代码，当导入包或该包中的模块时，执行`__init__.py`。

```python
# package folder
#   - __init__.py
import re
import sys
import os
```

```python
# test.py
import package
```
访问`package`文件中的引用文件需要加上包名。

`__all__`用来将模块全部导入

```
# __init__.py
__all__ = ['os', 'sys']
```

```
# test.py
from package import *
```

遇到`.pyo`文件不要慌，它就是`.pyc`优化后的字节码。

## import 杂谈

`__import__()`内置函数，强大但用处不大

```
_m = __import__('os' +'.' + 'path')
_m.curdir
```

`sys.path`模块搜索路径，第一个就是当前目录，可以自己改
```
sys.path.append('xxx/xxx')
```


`dir()`查询模块中定义的成员，用处不大，不如自己看文档

特殊文件`__init__.py`可以为空，也可以包含属于包的代码，当导入包或该包中的模块时，执行`__init__.py``。

## 命令行

- `sys.argv`参数
- `argparse`命令行选项解析

都不推荐使用，太麻烦了，推荐`fire`。
