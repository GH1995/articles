---
title: 明夷待访录（四）：Spark SQL
tags:
---



## Spark SQL简介

**从Shark说起**

Shark即Hive on Spark

**Spark SQL设计**

从HQL被解析成抽象语法树（AST）起，就全部由Spark SQL接管了


![Spark-SQL架构](https://s2.ax1x.com/2019/07/12/ZfYEss.jpg)

Spark SQL增å 了SchemaRDD（即带有Schema信息的RDD），使用户可以在Spark SQL中执行SQL语句，数据既可以来自RDD，也可以来自Hive、HDFS、Cassandra等外部数据源，还可以是JSON格式的数据。

![Spark-SQL支持的数据格式和编程语言](https://s2.ax1x.com/2019/07/12/ZfYMJU.jpg)

## DataFrame与RDD的区别

![DataFrame与RDD的区别](https://s2.ax1x.com/2019/07/12/ZfYXfU.jpg)

DataFrame的推出，让Spark具备了处理大规模结构化数据的能力

RDD是分布式的 Java对象的集合，比如，RDD[Person]是以Person为类型参数，但是，Person类的内部结构对于RDD而言却是不可知的。

DataFrame是一种以RDD为基础的分布式数据集，也就是分布式的Row对象的集合（每个Row对象代表一行记录），提供了详细的结构信息，也就是我们经常说的模式（schema），Spark SQL可以清楚地知道该数据集中包含哪些列、每列的名称和类型。

## DataFrame的创建

Spark使用全新的`SparkSession`接口替代Spark1.6中的`SQLContext`及`HiveContext`接口来实现其对数据加载、转换、处理等功能。

`SparkSession`支持从不同的数据源加载数据，并把数据转换成`DataFrame`，并且支持把`DataFrame`转换成`SQLContext`èª身中的表，然后使用SQL语句来操作数据。`SparkSessio`n亦提供了HiveQL以及其他依赖于Hive的功能的支持。

> 如何从people.json文件中读取数据并生成DataFrame并显示数据？


```python
from pyspark.sql import SparkSession
```


```python
spark = SparkSession.builder.getOrCreate()
```


```python
df = spark.read.json("file:///usr/local/spark/examples/src/main/resources/people.json")
```

看一下Json里面有什么


```python
df.show()
```

    +----+-------+
    | age|   name|
    +----+-------+
    |null|Michael|
    |  30|   Andy|
    |  19| Justin|
    +----+-------+
    


打印模式信息


```python
df.printSchema()
```

    root
     |-- age: long (nullable = true)
     |-- name: string (nullable = true)
    


选择多列


```python
df.select(df.name, df.age + 1).show()
```

    +-------+---------+
    |   name|(age + 1)|
    +-------+---------+
    |Michael|     null|
    |   Andy|       31|
    | Justin|       20|
    +-------+---------+
    


条件过滤


```python
df.filter(df.age > 20).show()
```

    +---+----+
    |age|name|
    +---+----+
    | 30|Andy|
    +---+----+
    


分组聚合


```python
df.groupBy("age").count().show()
```

    +----+-----+
    | age|count|
    +----+-----+
    |  19|    1|
    |null|    1|
    |  30|    1|
    +----+-----+
    


排序


```python
df.sort(df.age.desc()).show()
```

    +----+-------+
    | age|   name|
    +----+-------+
    |  30|   Andy|
    |  19| Justin|
    |null|Michael|
    +----+-------+
    


多列排序


```python
df.sort(df.age.desc(), df.name.asc()).show()
```

    +----+-------+
    | age|   name|
    +----+-------+
    |  30|   Andy|
    |  19| Justin|
    |null|Michael|
    +----+-------+
    


对列进行重命名


```python
df.select(df.name.alias("username"), df.age).show()
```

    +--------+----+
    |username| age|
    +--------+----+
    | Michael|null|
    |    Andy|  30|
    |  Justin|  19|
    +--------+----+
    


## 从RDD转换得到DataFrame

> 第一种方法是，利用反射来推断包含特定类型对象的RDD的schema，适用对已知数据结构的RDD转换；第二种方法是，使用编程接口，构造一个schema并将其应用在已知的RDD上。

### 利用反射机制推断RDD模式

在利用反射机制推断RDD模式时，我们会用到`toDF()`方法

反射机制，在我这里的理解很简单暴力：

- 通过字符串创建相应的对象
- 利用字符串的动态性，动态地创建需要的对象


```python
from pyspark.sql.types import Row
from pyspark import SparkContext
```


```python
sc = SparkContext("local", "test")
```


```python
def f(x):
    rel = {}
    rel['name'] = x[0]
    rel['age'] = x[1]

    return rel
```


```python
peopleDF = sc.textFile(
    "file:///usr/local/spark/examples/src/main/resources/people.txt").map(
        lambda line: line.split(',')).map(lambda x: Row(**f(x)).toDF())
```


```python
peopleDF.createOrReplaceTempView("people")
```


    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    <ipython-input-9-725c89f5f957> in <module>
    ----> 1 peopleDF.createOrReplaceTempView("people")
    

    AttributeError: 'PipelinedRDD' object has no attribute 'createOrReplaceTempView'



```python
peopleDF = spark.sql("select * from people")
```


```python
peopleDF.rdd.map(lambda t: "Name: " + t[0] + ", " + "Age: " + t[1]).take(10)
```




    ['Name:  29, Age: Michael', 'Name:  30, Age: Andy', 'Name:  19, Age: Justin']



**使用编程方式定义RDD模式**


```python
from pyspark.sql.types import StructType, StructField, StructType, StringType, Row
```

生成RDD


```python
peopleRDD = sc.textFile(
    "file:///usr/local/spark/examples/src/main/resources/people.txt")
```

定义一个模式字符串


```python
schemaString = "name age"
```

根据模式字符串生成模式


```python
fields = list(
    map(lambda fieldName: StructField(fieldName, StringType(), nullable=True),
        schemaString.split(" ")))
```


```python
schema = StructType(fields)
```


```python
schema
```




    StructType(List(StructField(name,StringType,true),StructField(age,StringType,true)))




```python
rowRDD = peopleRDD.map(lambda line: line.split(',')).map(
    lambda attributes: Row(attributes[0], attributes[1]))
```


```python
peopleDF = spark.createDataFrame(rowRDD, schema)
```


```python
peopleDF.createOrReplaceTempView("people")
```


```python
results.rdd.map(lambda attributes: "name: " + attributes[0] + "," + "age:" +
                attributes[1]).foreach(print)
```


```python
results.show()
```

    +-------+---+
    |   name|age|
    +-------+---+
    |Michael| 29|
    |   Andy| 30|
    | Justin| 19|
    +-------+---+
    


## 把RDD保存成文件


```python
peopleDF = spark.read.format("json").load(
    "file:///usr/local/spark/examples/src/main/resources/people.json")
```


```python
peopleDF.select("name", "age").write.format("csv").save(
    "file:///home/hadoop/spark/data/newpeople.csv")
```

另一种保存办法


```python
peopleDF.rdd.saveAsTextFile("file:///home/hadoop/spark/data/newpeople.txt")
```

## 读取和保存数据

### 读写 Parquet(DataFrame)

如何从parquet文件中加载数据生成DataFrame

这里需要把java版本降到1.8，然并卵

如何将DataFrame保存成parquet文件？


```python
peopleDF = spark.read.json(
    "file:///usr/local/spark/examples/src/main/resources/people.json")
```


```python
peopleDF.write.parquet("file:///home/hadoop/spark/data/newpeople.parquet")
```

### 通过 JDBC 连接数据库(DateFrame)

### 连接Hive读写数据



