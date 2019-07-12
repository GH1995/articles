---
title: '狸花猫记(2)：Spark 安装与使用'
date: 2019-06-17 19:39:39
tags:
    - spark
categories:
    - spark
---

# 安装和使用

## 安装 Hadoop 和 Spark

## 在 pyspark 中运行代码

## Spark 独立应用程序编程
```python
#!/usr/bin/env python3
# coding: utf-8

from pyspark import SparkContext

sc = SparkContext('local', 'test')

logFile = "file:///usr/local/spark/README.md"
logData = sc.textFile(logFile, 2).cache()

numAs = logData.filter(lambda line: 'a' in line).count()
numBs = logData.filter(lambda line: 'b' in line).count()

print('Lines with a: %s, Lines with b: %s' % (numAs, numBs))
```

# 第一个Spark应用程序：WordCount

## 在 pyspark 中执行词频统计

### 加载本地文件

```
textFile = sc.Te
```

# Spark集群环境搭建

# 在集群上运行Spark应用程序
