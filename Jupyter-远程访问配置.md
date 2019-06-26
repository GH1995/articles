---
title: Jupyter 远程访问配置
date: 2019-06-18 10:18:21
tags:
    - bugs
    - jupyter
---

目标：
在VPS上跑pyspark，spark的配置略过，主要讲jupyter方面。

jupyter配置的关键点在于找准错误，所有错误，看最上面一个就行，然后直接Google。我遇到的错误是，不能运行，具体是：

```
KeyError: 'allow_remote_access'
```

于是修改配置`~/.jupyter/jupyter_notebook_config.py`中

```python
c.NotebookApp.allow_remote_access = True
```

搞定！
