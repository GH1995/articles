---
title: 学林漫录：textwrap
date: 2019-06-24 10:19:53
tags:
    - Python
    - nip
---

这是Python自带的标准库，参见[这里](https://docs.python.org/3/library/textwrap.html)

## `textwrap.wrap(text, width=70, **kwargs)`

返回列表，每个元素的宽度为 `width`

![wrap](https://s2.ax1x.com/2019/06/24/ZkGEqA.png)


## `textwrap.fill(text, width=70, **kwargs)`
根据指定长度拆分字符串，然后逐行显示，结果为左对齐，第一行有缩进。行中的空格继续保留。

![fill](https://s2.ax1x.com/2019/06/24/Zk8DEt.png)

## `textwrap.indent(text, prefix, predicate=None)`

行首添加前缀，可以用来缩进

![indent](https://s2.ax1x.com/2019/06/24/ZkGyZR.png)


## `textwrap.dedent(text)`

每行取消缩进空格
