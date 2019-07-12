---
title: '狸花猫记(1)：Spark 设计和原理'
date: 2019-06-17 16:08:42
tags:
    - spark
categories:
    - spark
---

## 简介

![BDAS架构](http://dblab.xmu.edu.cn/blog/wp-content/uploads/2016/10/%E5%9B%BE-BDAS%E6%9E%B6%E6%9E%84.jpg)

Spark专注于数据的处理分析，而数据的存储还是要借助于Hadoop分布式文件系统HDFS、Amazon S3等来实现的

## 运行架构

### 基本概念

- RDD：弹性分布式数据集（Resilient Distributed Dataset）
- DAG：有向无环图
- Executor：运行在工作节点（Worker Node）上的一个进程，负责运行任务，并为应用程序存储数据

### 架构设计

Spark运行架构包括
- 集群资源管理器（Cluster Manager）
- 运行作业任务的工作节点（Worker Node）
- 每个应用的任务控制节点（Driver）
- 每个工作节点上负责具体任务的执行进程（Executor）

![运行架构](http://dblab.xmu.edu.cn/blog/wp-content/uploads/2016/11/%E5%9B%BE9-5-Spark%E8%BF%90%E8%A1%8C%E6%9E%B6%E6%9E%84.jpg)

一个应用（Application）
- 一个任务控制节点（Driver）
- 若干个作业（Job）
    - 一个作业由多个阶段（Stage）构成
        - 一个阶段由多个任务（Task）组成 

![Spark中各种概念之间的相互关系](http://dblab.xmu.edu.cn/blog/wp-content/uploads/2016/11/%E5%9B%BE9-6-Spark%E4%B8%AD%E5%90%84%E7%A7%8D%E6%A6%82%E5%BF%B5%E4%B9%8B%E9%97%B4%E7%9A%84%E7%9B%B8%E4%BA%92%E5%85%B3%E7%B3%BB.jpg)

### Spark运行基本流程

![Spark运行基本流程图](http://dblab.xmu.edu.cn/blog/wp-content/uploads/2016/11/%E5%9B%BE9-7-Spark%E8%BF%90%E8%A1%8C%E5%9F%BA%E6%9C%AC%E6%B5%81%E7%A8%8B%E5%9B%BE.jpg)

特点：
1. 每个应用都有自己专属的Executor进程
2. 只要能够获取Executor进程并保持通信
3. Executor上有一个BlockManager存储模块
4. 任务采用了数据本地性和推测执行等优化机制

## RDD的设计和运行原理

### 设计背景

矛盾：目前的MapReduce框架都是把中间结果写入到HDFS中，带来了大量的数据复制、磁盘IO和序列化开销

方法：提供了一个抽象的数据架构，我们不必担心底层数据的分布式特性，只需将具体的应用逻辑表达为一系列转换处理，不同RDD之间的转换操作形成依赖关系，可以实现管道化，从而避免了中间结果的存储，大大降低了数据复制、磁盘IO和序列化开销。

### RDD 概念

一个RDD就是一个分布式对象集合，本质上是**一个只读的分区记录集合**，每个RDD可以分成多个**分区**，每个分区就是一个**数据集片段**。

RDD提供了一组丰富的操作以支持常见的数据运算，分为**“行动”（Action）**和**“转换”（Transformation）**两种类型，前者用于**执行计算并指定输出的形式**，后者**指定RDD之间的相互依赖关系**。两类操作的主要区别是，**转换操作（比如map、filter、groupBy、join等）接受RDD并返回RDD**，而**行动操作（比如count、collect等）接受RDD但是返回非RDD**（即输出一个值或结果）。


RDD典型的执行过程如下：

1. RDD读入外部数据源（或者内存中的集合）进行创建；
2. RDD经过一系列的“转换”操作，每一次都会产生不同的RDD，供给下一个“转换”使用；
3. 最后一个RDD经“行动”操作进行处理，并输出到外部数据源（或者变成Scala集合或标量）。

![Spark的转换和行动操作](http://dblab.xmu.edu.cn/blog/wp-content/uploads/2016/11/%E5%9B%BE9-8-Spark%E7%9A%84%E8%BD%AC%E6%8D%A2%E5%92%8C%E8%A1%8C%E5%8A%A8%E6%93%8D%E4%BD%9C.jpg)

**血缘关系（Lineage）**

## RDD 特性

1. 高效的容错性
2. 中间结果持久化到内存
3. 存放的数据可以是Java对象，避免了不必要的对象序列化和反序列化开销

### RDD 之间的依赖关系

**独生子女**

如果父RDD的一个分区**只被一个子RDD的一个分区所使用**就是窄依赖，否则就是宽依赖。

窄依赖典型的操作包括`map`、`filter`、`union`等，宽依赖典型的操作包括`groupByKey`、`sortByKey`等。

![窄依赖和宽依赖](http://dblab.xmu.edu.cn/blog/wp-content/uploads/2016/11/%E5%9B%BE9-10-%E7%AA%84%E4%BE%9D%E8%B5%96%E4%B8%8E%E5%AE%BD%E4%BE%9D%E8%B5%96%E7%9A%84%E5%8C%BA%E5%88%AB.jpg)

### 阶段的划分

具体划分方法是：在DAG中进行反向解析，**遇到宽依赖就断开**，遇到窄依赖就把当前的RDD加入到当前的阶段中；将窄依赖尽量划分在同一个阶段中，可以实现流水线计算

![根据RDD分区的依赖关系划分阶段](http://dblab.xmu.edu.cn/blog/wp-content/uploads/2016/11/%E5%9B%BE9-11-%E6%A0%B9%E6%8D%AERDD%E5%88%86%E5%8C%BA%E7%9A%84%E4%BE%9D%E8%B5%96%E5%85%B3%E7%B3%BB%E5%88%92%E5%88%86%E9%98%B6%E6%AE%B5.jpg)

### RDD 运行过程

![RDD在Spark中的运行过程](http://dblab.xmu.edu.cn/blog/wp-content/uploads/2016/11/%E5%9B%BE9-12-RDD%E5%9C%A8Spark%E4%B8%AD%E7%9A%84%E8%BF%90%E8%A1%8C%E8%BF%87%E7%A8%8B.jpg)

1. 创建**RDD对象**；
2. SparkContext负责计算RDD之间的依赖关系，**构建DAG**；
3. DAG Scheduler负责把DAG图分解成多个**阶段**，每个阶段中包含了多个任务，每个**任务**会被任务调度器分发给各个**工作节点**（Worker Node）上的**Executor**去执行。

## 部署模式

### Spark三种部署方式

#### standalone模式

Spark与MapReduce1.0完全一致，都是由一个Master和若干个Slave构成，并且以槽（slot）作为资源分配单位

#### Spark on Mesos模式

Spark官方推荐采用这种模式，所以，许多公司在实际应用中也采用该模式。

#### Spark on YARN模式

![Spark on YARN架构](http://dblab.xmu.edu.cn/blog/wp-content/uploads/2016/11/%E5%9B%BE9-13-Spark-on-Yarn%E6%9E%B6%E6%9E%84.jpg)

### 从“Hadoop+Storm”架构转向Spark架构


“Hadoop+Storm”的架构（也称为Lambda架构）

Hadoop负责对批量历史数据的实时查询和离线分析，而Storm则负责对流数据的实时处理。

![采用“Hadoop+Storm”部署方式的一个案例](http://dblab.xmu.edu.cn/blog/wp-content/uploads/2016/11/%E5%9B%BE9-14-%E9%87%87%E7%94%A8HadoopStorm%E9%83%A8%E7%BD%B2%E6%96%B9%E5%BC%8F%E7%9A%84%E4%B8%80%E4%B8%AA%E6%A1%88%E4%BE%8B.jpg)

![用Spark架构同时满足批处理和流处理需求](http://dblab.xmu.edu.cn/blog/wp-content/uploads/2016/11/%E5%9B%BE9-15-%E7%94%A8Spark%E6%9E%B6%E6%9E%84%E6%BB%A1%E8%B6%B3%E6%89%B9%E5%A4%84%E7%90%86%E5%92%8C%E6%B5%81%E5%A4%84%E7%90%86%E9%9C%80%E6%B1%82.jpg)

### Hadoop和Spark的统一部署


![Hadoop和Spark的统一部署](http://dblab.xmu.edu.cn/blog/wp-content/uploads/2016/11/%E5%9B%BE9-16-Hadoop%E5%92%8CSpark%E7%9A%84%E7%BB%9F%E4%B8%80%E9%83%A8%E7%BD%B2.jpg)
