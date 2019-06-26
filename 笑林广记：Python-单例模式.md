---
title: '笑林广记：Python 单例模式 '
date: 2019-06-25 15:24:46
tags:
    - Python
---


> Python 的单例模式如何实现？


单例模式，应当是每个程序员都需要知道的一种设计模式，它简单、高效。而Python 的单例模式实现使用都尤为简单。

```
class Singleton(object): 
    pass

singleton = Singleton()
```

将上面的代码在 `.py` 文件中，在用的地方导入的 `singleton` 就是单例的。单例存在哪里呢？就在那个`.pyc`文件。第一次导入时，会生成 `.pyc` 文件，当第二次导入时，就会直接加载 `.pyc` 文件。

使用方法
```
from a import singleton
```

