---
title: '明夷待访录（五）：Spark Steaming '
tags:
---


# Spark Streaming

## 流计算简介

![流计算处理过程](https://s2.ax1x.com/2019/07/12/Zf41pD.jpg)

- 数据实时采集
    - Facebook的Scribe
    - LinkedIn的Kafka
    - 淘宝的TimeTunnel
    - 基于Hadoop的Chukwa和Flume等
- 数据实时计算
- 实时查询服务

## Spark Streaming简介

**Spark Streaming设计**

![Spark-Streaming支持的输入、输出数据源](https://s2.ax1x.com/2019/07/12/Zfoojg.jpg)

Spark Streaming的基本原理是将实时输入数据流以时间片（秒级）为单位进行拆分，然后经Spark引擎以类似批处理的方式处理每个时间片数据。

Spark Streaming最主要的抽象是DStream（Discretized Stream，离散化数据流），表示连续不断的数据流。

在内部实现上，Spark Streaming的输入数据按照时间片（如1秒）分成一段一段的DStream，每一段数据转换为Spark中的RDD，并且对DStream的操作都最终转变为对相应的RDD的操作。

**Spark Streaming与Storm的对比**

Spark Streaming无法实现毫秒级的流计算，而Storm可以实现毫秒级响应。

## DStream操作概述

Spark Streaming通过input DStream与外部数据源进行连接，读取相关数据。

**Spark Streaming程序基本步骤**

1. 通过创建输入`DStream`来定义输入源
2. 通过对`DStream`应用转换操作和输出操作来定义流计算。
3. 用`streamingContext.start()`来开始接收数据和处理流程。
4. 通过`streamingContext.awaitTermination()`方法来等待处理结束（手动结束æ因为错误而结束）。
5. 可以通过`streamingContext.stop()`来手动结束流计算进程。

**创建StreamingContext对象**

`StreamingContext`对象，它是Spark Streaming程序的主入口


```python
from pyspark import SparkContext
from pyspark.streaming import StreamingContext
```


```python
sc = SparkContext("local", "test")
```


```python
ssc = StreamingContext(sc, 1)  # 1表示每隔1秒钟就自动执行一次流计算
```

如果是编写一个独立的 Spark Streaming 程序，而不是在 pyspark 中运行


```python
from pyspark import SparkContext, SparkConf
from pyspark.streaming import StreamingContext
```


```python
conf = SparkConf()
conf.setAppName('TestDStream')
conf.setMaster('local[1]')
```




    <pyspark.conf.SparkConf at 0x7fda18e5e860>




```python
sc = SparkContext(conf=conf)
ssc = StreamingContext(sc, 1)
```

### 输入源

#### 基本输入源

#### 文件流


```python
#!/usr/bin/env python3
# coding: utf-8

from operator import add
from pyspark import SparkContext, SparkConf
from pyspark.streaming import StreamingContext

conf = SparkConf()
conf.setAppName('TestDStream')
conf.setMaster('local[1]')

sc = SparkContext(conf=conf)
ssc = StreamingContext(sc, 20)

lines = ssc.textFileStream('file:///home/hadoop/spark/src/test/streaming/logfile')  # 这里监听的是文件夹

words = lines.flatMap(lambda line: line.split(' '))
wordCounts = words.map(lambda x: (x, 1)).reduceByKey(add)
wordCounts.pprint()

ssc.start()
ssc.awaitTermination()
```

**套接字流**


```python
#!/usr/bin/env python3
# coding: utf-8

from __future__ import print_function

import sys

from pyspark import SparkContext
from pyspark.streaming import StreamingContext

if __name__ == "__main__":
    if len(sys.argv) != 3:
        print("Usage: netword_wordcount.py <hostname> <port>", file=sys.stderr)
        exit(-1)

    sc = SparkContext(appName="PythonStreamingNetworkWordCount")
    ssc = StreamingContext(sc, 1)

    lines = ssc.socketTextStream(sys.argv[1], int(sys.argv[2]))
    counts = lines.flatMap(lambda line: line.split(" "))\
        .map(lambda word: (word, 1))\
        .reduceByKey(lambda a, b: a+b)
    counts.pprint()


    ssc.start()
    ssc.awaitTermination()
```

**RDD 队列流**


```python
#!/usr/bin/env python3
# coding: utf-8

import time
from pyspark import SparkContext
from pyspark.streaming import StreamingContext

if __name__ == "__main__":
    sc = SparkContext(appName="PythonStreamingQueueStream")
    ssc = StreamingContext(sc, 1)

    rddQueue = []
    for i in range(5):
        rddQueue += [ssc.sparkContext.parallelize([j for j in range(1, 1001)], 10)]

    inputStream = ssc.queueStream(rddQueue)
    mappedStream = inputStream.map(lambda x: (x % 10, 1))
    reducedSrteam = mappedStream.reduceByKey(lambda a, b: a+b)

    reducedSrteam.pprint()

    ssc.start()
    time.sleep(6)
    ssc.stop(stopSparkContext=True, stopGraceFully=True)

```

#### 高级数据源

##### Kafka

Kafka是一种高吞吐量的分布式发布订阅消息系统，它可以处理消费者规模的网站中的所有动作流数据。Kafka的目的是通过Hadoop的并行加载机制来统一线上和离线的消息处理，也是为了通过集群机来提供实时的消费。

**核心概念**

- Broker Kafka集群包含一个或多个服务器，这种服务器被称为broker
- Topic 每条发布到Kafka集群的消息都有一个类别，这个类别被称为Topic
- Partition 是物理上的概念，每个Topic包含一个或多个Partition
- Producer 负责发布消息到Kafka broker
- Consumer 消息消费者，向Kafka broker读取消息的客户端 
- Consumer Group 每个Consumer属于一个特定的Consumer Group

##### Flume

### 转换操作

DStream转换操作包括无状态转换和有状态转换。

- **无状态转换**：每个批次的处理不依赖于之前批次的数据。
- **有状态转换**：当前批次的处理需要使用之前批次的数据或者中间结果。有状态转换包括基于滑动窗口的转换和追踪状态变化的转换(updateStateByKey)。

**DStream无状态转换操作**

**DStream有状态转换操作**

滑动窗口转换操作

updateStateByKey操作

### 输出操作

