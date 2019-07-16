---
title: 学林漫录：美化数据输出——pprint
tags:
  - Python
categories:
  - python
date: 2019-06-25 18:15:37
---

美化数据输出的一个包，对我来说最大的用处就是控制`depth`

```
data = [(1,{'a':'A','b':'B','c':'C','d':'D'}),
    (2,{'e':'E','f':'F','g':'G','h':'H','i':'I','j':'J','k':'K','l':'L'})]
```

```
import pprint

pprint.pprint(data)
pprint.pprint(data, depth = 2)
```
