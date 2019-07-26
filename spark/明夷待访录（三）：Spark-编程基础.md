---
title: 明夷待访录（三）：Spark 编程基础
tags:
    - spark
categories:
    - spark
---


## RDD编程

**RDD创建**

**创建办法**

- 读取外部数据
    - 本地文件
    - HDFS
    - HBase
    - Cassandra等
- 调用 `SparkContext` 的 `parallelize` 方法，在 Driver 中一个已经存在的集合（数组）上创建

**创建RDD之前的准备工作**

启动 HDFS 组件

```sh
/usr/local/hadoop/sbin/start-dfs.sh
```

创建 rdd 子目录存放代码和相关文件


```python
!mkdir rdd && cd rdd
```

在rdd目录下新建一个word.txt文件，随便输入什么内容。这里我直接放的就是 man 手册

**从文件系统中加载数据创建RDD**

`SparkContext`创建上下文对象


```python
from pyspark import SparkContext

sc = SparkContext("local", "test")
```

三条命令都等价

`textFile()` 从文件系统中加载数据创建`rdd`，参数是URI
- 本地文件系统地址
- 分布式文件系统 HDFS 地址

这里我们不用上面创建的word.txt，而是直接用 README.md，保证读者和文章输出一致。

`textFile()` 的第二个参数用来指定分区数目，默认是最小值 128MB。


```python
lines = sc.textFile("hdfs://localhost:9000/user/hadoop/README.md")
lines = sc.textFile("/user/hadoop/README.md")
lines = sc.textFile("README.md")
```

**通过并行集合（数组）创建RDD**

可以调用SparkContext的parallelize方法，在Driver中一个已经存在的集合（数组）上创建。


```python
nums = [1, 2, 3, 4, 5]
rdd = sc.parallelize(nums)
```

**RDD操作**

> 转换
> 
> 转换过程只是记录了转换的轨迹，并不会发生真正的计算  
> 这里要注意，他们返回的是什么东西


 * `filter(func)`：筛选出满足函数`func`的元素，并返回一个新的数据集
 * `map(func)`：将每个元素传递到函数`func`中，并将结果返回为一个新的数据集
 * `flatMap(func)`：与`map()`相似，但每个输入元素都可以映射到0或多个输出结果
 * `groupByKey()`：应用于`(K,V)`键值对的数据集时，返回一个新的`(K, Iterable)`形式的数据集
 * `reduceByKey(func)`：应用于`(K,V)`键值对的数据集时，返回一个新的`(K, V)`形式的数据集，其中的每个值是将每个`key`传递到函数`func`中进行聚合

- `fiter()` 返回的是类似于`list`，`list`里面是满足条件的数据，这个`list`相对于原始的`list`变短了
- `map()` 返回的是`list_transform`，它是`list`的变形，和`list`一一对应
- `flatMap()` 返回`list_transform`，但是不是一一对应，可以**一对多**或者**一对零**
- `groupByKey()` 收集所有 `K` 相同的 `V`，返回的数据类似于`defaultdict(list)`
- `reduceByKey()` **将 `K`分组再进行reduce，$v_1 \text{ op } v_2 \to v_1$**

> 行动
>
> 行动操作是真正触发计算的地方  
> `take` 比 `print` 好用

* `count()` 返回数据集中的元素个数
* `collect()` 以数组的形式返回数据集中的所有元素，返回一个**数组**
* `first()` 返回数据集中的第一个元素
* `take(n)` 以数组的形式返回数据集中的前n个元素
* `reduce(func)` 通过函数func（输入两个参数并返回一个值）聚合数据集中的元素
* `foreach(func)` 将数据集中的每个元素传递到函数func中运行

> 注意  
> `map`是转换，不会实际操作  
> `reduce`是动作，它是来真的  

**惰性机制**

`lines` 读取

`textFile()` 只是一个转换操作，并不会真的去读文件 


```python
lineLength = lines.map(lambda s: len(s))  # 计算每行的长度（即每行包含多少个单词）
```


```python
totalLength = lineLength.reduce(lambda a, b: a + b) 
```


```python
totalLength
```




    3847



Spark会把计算分解成多个任务在不同的机器上执行，每台机器运行位于属于它自己的map和reduce，最后把结果返回给Driver Program。

**实例**

计算结果集中满足条件的元素个数

将当前遍历到的行赋值给参数`line`，然后对每行文本执行lamda表达式，满足条件的`line`被放入结果集中。最后执行 `count`。


```python
lines.filter(lambda line: "Spark" in line).count()
```




    20



找出文本文件中**单行文本所包含的单词数量**的最大值

- `lambda line: len(line.split(" "))` 将每行文本传递给lambda，计算出每行文本的单词数，得到是一个rdd，每个元素都是整数
- `lambda a, b: (a > b and a or b)` 每次接收两个参数，留下较大者。这里是个 trick


```python
lines.map(lambda line: len(line.split(" "))).reduce(lambda a, b: (a > b and a
                                                                  or b))
```




    22



**自己写的，看单词的最大长度**


```python
lines.flatMap(lambda line: line.split(" ")).map(lambda word: len(word)).reduce(
    lambda a, b: (a > b and a or b))
```




    111



**持久化**

两次操作触发了两次从头到尾的计算


```python
list = ["Hadoop", "Spark", "Hive"]
rdd = sc.parallelize(list)
print(rdd.count())
```

    3



```python
print(','.join(rdd.collect()))
```

    Hadoop,Spark,Hive


`persist()`方法对一个RDD标记为持久化，之所以说“标记为持久化”，是因为出现`persist()`语句的地方，并不会马上计算生成RDD并把它持久化，而是要等到遇到第一个行动操作触发真正计算以后，才会把计算结果进行持久化，持久化后的RDD将会被保留在计算节点的内存中被后面的行动操作重复使用。


```python
list = ["Hadoop", "Spark", "Hive"]
rdd = sc.parallelize(list)
```


```python
rdd.cache() # 会调用persist(MEMORY_ONLY)，但是，语句执行到这里，并不会缓存rdd，这时rdd还没有被计算生成
```




    ParallelCollectionRDD[9] at parallelize at PythonRDD.scala:195



一般而言，使用`cache()`方法时，会调用`persist(MEMORY_ONLY)`。


```python
print(rdd.count()) # 这时才会执行上面的rdd.cache()，把这个rdd放到缓存中
```

    3



```python
print(', '.join(rdd.collect())) # 不需要触发从头到尾的计算，只需要重复使用上面缓存中的rdd
```

    Hadoop, Spark, Hive


可以使用`unpersist()`方法手动地把持久化的RDD从缓存中移除。

**分区**

RDD是弹性分布式数据集，通常RDD很大，会被分成很多个分区，分别保存在不同的节点上。

RDD分区的一个分区原则是使得分区的个数尽量等于集群中的CPU核心（core）数目。

`spark.default.parallelism`配置默认的分区数

Spark 的四种部署模式

- 本地模式 log[N]
- Standalone模式，集群中所有CPU核心数目总和，但不小于2
- YARN 模式，集群中所有CPU核心数目总和，但不小于2
- Mesos模型，默认为 8


```python
array = [1, 2, 3, 4, 5]
```


```python
rdd = sc.parallelize(array, 2)  #设置两个分区
```

**打印元素**

本地`rdd.foreach(print)`或者`rdd.map(print)`

多机`rdd.collect().foreach(print)`或`rdd.take(100).foreach(print)`

**存在问题**


```python
rdd.foreach(print) # 无法使用
```

**键值对RDD**

**键值对RDD的创建 pairrdd**

第一种创建方式：从文件中加载


```python
# lines = sc.textFile(path)
```




    /user/hadoop/README.md MapPartitionsRDD[1] at textFile at NativeMethodAccessorImpl.java:0




```python
pairRDD = lines.flatMap(lambda line: line.split(" "))\
            .map(lambda word: (word, 1))
# 单词集合
# lambda word: (word, 1) -> 用 tuple 创建 pair
```


```python
pairRDD.take(10)
```




    [('#', 1),
     ('Apache', 1),
     ('Spark', 1),
     ('', 1),
     ('Spark', 1),
     ('is', 1),
     ('a', 1),
     ('fast', 1),
     ('and', 1),
     ('general', 1)]



第二种创建方式：通过并行集合（列表）创建RDD


```python
list = ["Hadoop", "Spark", "Hive", "Spark"]
rdd = sc.parallelize(list)
```


```python
pairRDD = rdd.map(lambda word: (word, 1))
```


```python
pairRDD.take(10)
```




    [('Hadoop', 1), ('Spark', 1), ('Hive', 1), ('Spark', 1)]



**常用的键值对转换操作**

`reduceByKey(func)` 使用 `func` 函数合并具有相同键的值

`(a,b) => a+b `这个Lamda表达式中，**a和b都是指value**


```python
pairRDD.reduceByKey(lambda a, b: a + b).take(10)
```




    [('Hadoop', 1), ('Spark', 2), ('Hive', 1)]



**groupByKey()**的功能是，对具有相同键的值进行group


```python
pairRDD.groupByKey().take(10)
```




    [('Hadoop', <pyspark.resultiterable.ResultIterable at 0x7faaea1ba470>),
     ('Spark', <pyspark.resultiterable.ResultIterable at 0x7faaea1ba400>),
     ('Hive', <pyspark.resultiterable.ResultIterable at 0x7faaea1ba5c0>)]



- `keys()`只会把键值对RDD中的key返回形成一个新的RDD

采用`keys()`后得到的结果是一个`RDD[Int]`，内容是{"spark","spark","hadoop","hadoop"}


```python
pairRDD.keys().take(10)
```




    ['Hadoop', 'Spark', 'Hive', 'Spark']



- `values()` values()只会把键值对RDD中的value返回形成一个新的RDD

采用`values()`后得到的结果是一个`RDD[Int]`，内容是{1,2,3,5}


```python
pairRDD.values().take(10)
```




    [1, 1, 1, 1]



**`sortByKey()`的功能是返回一个根据键排序的RDD**


```python
pairRDD.sortByKey().take(10)
```




    [('Hadoop', 1), ('Hive', 1), ('Spark', 1), ('Spark', 1)]



**`mapValues(func)`**

我们只想对键值对RDD的value部分进行处理，而不是同时对key和value进行处理

**对键值对RDD中的每个value都应用一个函数**


```python
pairRDD.mapValues(lambda x: x + 1).take(10)
```




    [('Hadoop', 2), ('Spark', 2), ('Hive', 2), ('Spark', 2)]



**`join`表示内连接**

对于内连接，对于给定的两个输入数据集`(K,V1)`和`(K,V2)`，只有在两个数据集中都存在的key才会被输出，最终得到一个`(K,(V1,V2))`类型的数据集


```python
pairRDD1 = sc.parallelize([('spark', 1), ('spark', 2), ('hadoop', 3),
                           ('hadoop', 5)])

pairRDD2 = sc.parallelize([('spark', 'fast')])
```


```python
pairRDD1.join(pairRDD2).take(10)
```




    [('spark', (1, 'fast')), ('spark', (2, 'fast'))]




```python
pairRDD2.join(pairRDD1).take(10)
```




    [('spark', ('fast', 1)), ('spark', ('fast', 2))]



**一个综合实例**

题目：给定一组键值对("spark",2),("hadoop",6),("hadoop",4),("spark",6)

键值对的key表示图书名称，value表示某天图书销量

请计算每个键对应的平均值，也就是计算每种图书的每天平均销量。


```python
rdd = sc.parallelize([("spark", 2), ("hadoop", 6), ("hadoop", 4),
                      ("spark", 6)])  # rdd 类型是 RDD[(string, int)]
```


```python
rdd.mapValues(lambda x: (x, 1)) \
    .reduceByKey(lambda x, y: (x[0] + y[0], x[1] + y[1]))\
    .mapValues(lambda x: (x[0] / x[1])) \
    .collect()
```




    [('spark', 4.0), ('hadoop', 5.0)]



1. `mapVlaues(x, 1)` 打散数据（金额，数量）
2. `reduceByKeys` 分桶计算总金额和总数量，这里没有 key 的事情了
3. `mapValues` 除法得到平均金额

键值对("spark",2)经过mapValues()函数处理后，就变成了("spark",(2,1))，其中，数值1表示"spark"这个键的1次出现

x和y都是value，而且是具有相同key的两个键值对所对应的value，比如，在这个例子中， `("hadoop",(6,1))`和`("hadoop",(4,1))`这两个键值对具有相同的key，所以，对于函数中的输入参数(x,y)而言，x就是(6,1)，序列从0开始计算，`x[0]`表示这个键值对中的第1个元素6，`x[1]`表示这个键值对中的第二个元素1，y就是(4,1)，`y[0]`表示这个键值对中的第1个元素4，`y[1]`表示这个键值对中的第二个元素1，所以，函数体`(x[0]+y[0],x[1] + y[2])`，相当于生成一个新的键值对`(key,value)`，其中，key是`x[0]+y[0]`，也就是6+4=10，value是`x[1] + y[1]`，也就是1+1=2，因此，函数体`(x[0]+y[0],x[1] + y[1])`执行后得到的value是(10,2)

## 共享变量

需要在多个任务之间共享变量，或者在任务（Task）和任务控制节点（Driver Program）之间共享变量

广播变量用来把变量在所有节点的内存之间进行共享。累加器则支持在所有不同节点之间进行累加计算（比如计数或者求和）。

**广播变量**


Spark的 Action 操作会跨越多个阶段（stage），对于每个阶段内的所有任务所需要的公共数据，Spark都会自动进行广播。通过广播方式进行传播的变量，会经过序列化，然后在被任务使用时再进行反序列化。这就意味着，显式地创建广播变量只有在下面的情形中是有用的：当跨越多个阶段的那些任务需要相同的数据，或者当以反序列化方式对数据进行缓存是非常重要的。

通过调用`SparkContext.broadcast(v)`来从一个普通变量`v`中创建一个广播变量。这个广播变量就是对普通变量`v`的一个包装器，通过调用`value`方法就可以获得这个广播变量的值


```python
broadcastVar = sc.broadcast([1, 2, 3])
```


```python
broadcastVar.value
```




    [1, 2, 3]



一旦广播变量创建后，普通变量v的值就不能再发生修改，从而确保所有节点都获得这个广播变量的相同的值。

**累加器**

用来实现计数器（counter）和求和（sum）

通过调用`SparkContext.accumulator()`来创建

运行在集群中的任务，就可以使用`add`方法来把数值累加到累加器上，但是，这些任务只能做累加操作，不能读取累加器的值，只有任务控制节点（Driver Program）可以使用value方法来读取累加器的值。


```python
accum = sc.accumulator(0)
```


```python
sc.parallelize([1, 2, 3, 4]).foreach(lambda x: accum.add(x))
accum.value
```




    10



## 数据读写

### 文件系统的数据读写

介绍文件系统的读写和 HDFS 的读写

**本地文件系统的数据读写**


```python
textFile = sc.textFile("file:///usr/local/spark/README.md")
```


```python
textFile.first()
```




    '# Apache Spark'




```python
textFile.saveAsTextFile("file:///home/hadoop/spark/data/wordcount/writeback.txt")
```


```python
!ls /home/hadoop/spark/data/wordcount/writeback.txt/
```

    part-00000  _SUCCESS


**分布式文件系统HDFS的数据读写**

查看HDFS文件系统根目录下的内容

**这里的几条命令很重要**


```python
!/usr/local/hadoop/bin/hdfs dfs -ls /
```

    Found 1 items
    drwxr-xr-x   - hadoop supergroup          0 2019-06-18 15:48 /user


```sh
./bin/hdfs dfs -ls /user/hadoop
./bin/hdfs dfs -put /usr/local/spark/mycode/wordcount/word.txt .
./bin/hdfs dfs -cat ./word.txt
```


```python
textFile = sc.textFile("hdfs://localhost:9000/user/hadoop/README.md")
```


```python
textFile.first()
```




    '# Apache Spark'



```
>>> val textFile = sc.textFile("hdfs://localhost:9000/user/hadoop/word.txt")
>>> val textFile = sc.textFile("/user/hadoop/word.txt")
>>> val textFile = sc.textFile("word.txt")
```

```
>>> val textFile = sc.textFile("word.txt")
>>> textFile.saveAsTextFile("writeback.txt")
```

**不同文件格式的读写**

- 文本文件

- `sc.textFile`
- ` rdd.saveAsTextFile`

- JSON


```python
jsonStr = sc.textFile(
    "file:///usr/local/spark/examples/src/main/resources/people.json")
```


```python
jsonStr.take(10)
```




    ['{"name":"Michael"}',
     '{"name":"Andy", "age":30}',
     '{"name":"Justin", "age":19}']




```python
import json
```


```python
result = jsonStr.map(lambda s: json.loads(s))
```


```python
result.take(10)
```




    [{'name': 'Michael'},
     {'name': 'Andy', 'age': 30},
     {'name': 'Justin', 'age': 19}]



### 文件数据读写

### 读写HBase数据

HBase是针对谷歌BigTable的开源实现，是一个高可靠、高性能、面向列、可伸缩的分布式数据库，主要用来存储非结构化和半结构化的松散数据。HBase可以支持超大规模数据存储，它可以通过水平扩展的方式，利用廉价计算机集群处理由超过10亿行数据和数百万列元素组成的数据表。

