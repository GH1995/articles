---
title: 给忙碌烦躁者的基本魔咒：absolute_import
date: 2019-06-27 11:55:32
tags:
    - Python
    - 常识
---

```
from __future__ import absolute_import
```

禁止`import string`查找当前目录下文件

Python行为变成用`import string`来引入系统的标准`string.py`，而用`from pkg import string`来引入当前目录下的`string.py`了
