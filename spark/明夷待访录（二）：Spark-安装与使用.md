---
title: '明夷待访录（二）：Spark 安装与使用'
date: 2019-06-17 19:39:39
tags:
    - spark
categories:
    - spark
---

# 安装和使用

## 安装 Hadoop 和 Spark

> 这里注意 openjdk 的版本必须是 8！

1. 安装 java，配置`JAVA_HOME`
```
export JAVA_HOME=JDK安装路径
```
2. 安装 `Hadoop`到 `/usr/local/hadoop`
3. 修改配置文件`core-site.xml`和`hdfs-site.xml`
4. 启动HDFS
```
/usr/local/hadoop/sbin/start-dfs.sh
```
停止是
```
/usr/local/hadoop/sbin/stop-all.sh
```


1. Spark 安装到`/usr/local/spark`
2. 修改配置文件`cp spark-env.sh.template spark-env.sh`，在行首添加`export SPARK_DIST_CLASSPATH=$(/usr/local/hadoop/bin/hadoop classpath)`
3. 在`.bashrc`中写入`HADOOP_HOME` `SPARK_HOME` 

## 在 pyspark 中运行代码

conda 安装，直接启动即可

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

## Spark的安装与使用

### 第一个Spark应用程序：WordCount

### 任务要求


```python
! mkdir wordcount
```


```python
! cd wordcount
```


```python
!pwd
```

    /home/hadoop/spark/src/test


### 在pyspark中执行词频统计

#### 启动pyspark


```python
!pyspark
```

    Python 3.7.3 (default, Mar 27 2019, 22:11:17) 
    [GCC 7.3.0] :: Anaconda, Inc. on linux
    Type "help", "copyright", "credits" or "license" for more information.
    2019-06-18 16:39:00,498 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
    Setting default log level to "WARN".
    To adjust logging level use sc.setLogLevel(newLevel). For SparkR, use setLogLevel(newLevel).
    Welcome to
          ____              __
         / __/__  ___ _____/ /__
        _\ \/ _ \/ _ `/ __/  '_/
       /__ / .__/\_,_/_/ /_/\_\   version 2.4.3
          /_/
    
    Using Python version 3.7.3 (default, Mar 27 2019 22:11:17)
    SparkSession available as 'spark'.
    >>> 
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
      File "/usr/local/spark/python/pyspark/context.py", line 270, in signal_handler
        raise KeyboardInterrupt()
    KeyboardInterrupt
    >>> 

#### 加载本地文件


```python
from pyspark import SparkContext
```


```python
sc = SparkContext("local", "test")
```


```python
textFile = sc.textFile("file:///usr/local/spark/README.md")
```


```python
textFile.first()
```




    '# Apache Spark'




```python
textFile.saveAsTextFile("file:///home/hadoop/spark/data/wordcount/writeback")
```

加载HDFS文件


```python
!/usr/local/hadoop/bin/hdfs dfs -ls .
```

    Found 2 items
    drwxr-xr-x   - hadoop supergroup          0 2019-06-18 15:48 input
    drwxr-xr-x   - hadoop supergroup          0 2019-06-18 15:49 output


查看Linux当前登录用户hadoop在HDFS文件系统中与hadoop对应的目录下的文件


```python
!/usr/local/hadoop/bin/hdfs dfs -ls /
```

    Found 1 items
    drwxr-xr-x   - hadoop supergroup          0 2019-06-18 15:48 /user


上传到分布式文件系统HDFS中


```python
!/usr/local/hadoop/bin/hdfs dfs -put /usr/local/spark/README.md .
```


```python
!/usr/local/hadoop/bin/hdfs dfs -ls .
```

    Found 3 items
    -rw-r--r--   1 hadoop supergroup       3952 2019-06-18 16:48 README.md
    drwxr-xr-x   - hadoop supergroup          0 2019-06-18 15:48 input
    drwxr-xr-x   - hadoop supergroup          0 2019-06-18 15:49 output



```python
!/usr/local/hadoop/bin/hdfs dfs -cat ./README.md
```

    # Apache Spark
    
    Spark is a fast and general cluster computing system for Big Data. It provides
    high-level APIs in Scala, Java, Python, and R, and an optimized engine that
    supports general computation graphs for data analysis. It also supports a
    rich set of higher-level tools including Spark SQL for SQL and DataFrames,
    MLlib for machine learning, GraphX for graph processing,
    and Spark Streaming for stream processing.
    
    <http://spark.apache.org/>
    
    
    ## Online Documentation
    
    You can find the latest Spark documentation, including a programming
    guide, on the [project web page](http://spark.apache.org/documentation.html).
    This README file only contains basic setup instructions.
    
    ## Building Spark
    
    Spark is built using [Apache Maven](http://maven.apache.org/).
    To build Spark and its example programs, run:
    
        build/mvn -DskipTests clean package
    
    (You do not need to do this if you downloaded a pre-built package.)
    
    You can build Spark using more than one thread by using the -T option with Maven, see ["Parallel builds in Maven 3"](https://cwiki.apache.org/confluence/display/MAVEN/Parallel+builds+in+Maven+3).
    More detailed documentation is available from the project site, at
    ["Building Spark"](http://spark.apache.org/docs/latest/building-spark.html).
    
    For general development tips, including info on developing Spark using an IDE, see ["Useful Developer Tools"](http://spark.apache.org/developer-tools.html).
    
    ## Interactive Scala Shell
    
    The easiest way to start using Spark is through the Scala shell:
    
        ./bin/spark-shell
    
    Try the following command, which should return 1000:
    
        scala> sc.parallelize(1 to 1000).count()
    
    ## Interactive Python Shell
    
    Alternatively, if you prefer Python, you can use the Python shell:
    
        ./bin/pyspark
    
    And run the following command, which should also return 1000:
    
        >>> sc.parallelize(range(1000)).count()
    
    ## Example Programs
    
    Spark also comes with several sample programs in the `examples` directory.
    To run one of them, use `./bin/run-example <class> [params]`. For example:
    
        ./bin/run-example SparkPi
    
    will run the Pi example locally.
    
    You can set the MASTER environment variable when running examples to submit
    examples to a cluster. This can be a mesos:// or spark:// URL,
    "yarn" to run on YARN, and "local" to run
    locally with one thread, or "local[N]" to run locally with N threads. You
    can also use an abbreviated class name if the class is in the `examples`
    package. For instance:
    
        MASTER=spark://host:7077 ./bin/run-example SparkPi
    
    Many of the example programs print usage help if no params are given.
    
    ## Running Tests
    
    Testing first requires [building Spark](#building-spark). Once Spark is built, tests
    can be run using:
    
        ./dev/run-tests
    
    Please see the guidance on how to
    [run tests for a module, or individual tests](http://spark.apache.org/developer-tools.html#individual-tests).
    
    There is also a Kubernetes integration test, see resource-managers/kubernetes/integration-tests/README.md
    
    ## A Note About Hadoop Versions
    
    Spark uses the Hadoop core library to talk to HDFS and other Hadoop-supported
    storage systems. Because the protocols have changed in different versions of
    Hadoop, you must build Spark against the same version that your cluster runs.
    
    Please refer to the build documentation at
    ["Specifying the Hadoop Version and Enabling YARN"](http://spark.apache.org/docs/latest/building-spark.html#specifying-the-hadoop-version-and-enabling-yarn)
    for detailed guidance on building for a particular distribution of Hadoop, including
    building for particular Hive and Hive Thriftserver distributions.
    
    ## Configuration
    
    Please refer to the [Configuration Guide](http://spark.apache.org/docs/latest/configuration.html)
    in the online documentation for an overview on how to configure Spark.
    
    ## Contributing
    
    Please review the [Contribution to Spark guide](http://spark.apache.org/contributing.html)
    for information on how to get started contributing to the project.



```python
textFile.saveAsTextFile("writeback")
```


```python
!/usr/local/hadoop/bin/hdfs dfs -ls .
```

    Found 4 items
    -rw-r--r--   1 hadoop supergroup       3952 2019-06-18 16:48 README.md
    drwxr-xr-x   - hadoop supergroup          0 2019-06-18 15:48 input
    drwxr-xr-x   - hadoop supergroup          0 2019-06-18 15:49 output
    drwxr-xr-x   - hadoop supergroup          0 2019-06-18 16:50 writeback



```python
!/usr/local/hadoop/bin/hdfs dfs -ls ./writeback
```

    Found 2 items
    -rw-r--r--   1 hadoop supergroup          0 2019-06-18 16:50 writeback/_SUCCESS
    -rw-r--r--   1 hadoop supergroup       3952 2019-06-18 16:50 writeback/part-00000


词频统计


```python
wordCount = textFile.flatMap(lambda line: line.split(" ")).map(lambda word: (word,1)).reduceByKey(lambda a, b : a + b)
```


```python
wordCount.collect()
```




    [('#', 1),
     ('Apache', 1),
     ('Spark', 16),
     ('', 72),
     ('is', 7),
     ('a', 9),
     ('fast', 1),
     ('and', 10),
     ('general', 3),
     ('cluster', 2),
     ('computing', 1),
     ('system', 1),
     ('for', 12),
     ('Big', 1),
     ('Data.', 1),
     ('It', 2),
     ('provides', 1),
     ('high-level', 1),
     ('APIs', 1),
     ('in', 6),
     ('Scala,', 1),
     ('Java,', 1),
     ('Python,', 2),
     ('R,', 1),
     ('an', 4),
     ('optimized', 1),
     ('engine', 1),
     ('that', 2),
     ('supports', 2),
     ('computation', 1),
     ('graphs', 1),
     ('data', 1),
     ('analysis.', 1),
     ('also', 5),
     ('rich', 1),
     ('set', 2),
     ('of', 5),
     ('higher-level', 1),
     ('tools', 1),
     ('including', 4),
     ('SQL', 2),
     ('DataFrames,', 1),
     ('MLlib', 1),
     ('machine', 1),
     ('learning,', 1),
     ('GraphX', 1),
     ('graph', 1),
     ('processing,', 1),
     ('Streaming', 1),
     ('stream', 1),
     ('processing.', 1),
     ('<http://spark.apache.org/>', 1),
     ('##', 9),
     ('Online', 1),
     ('Documentation', 1),
     ('You', 4),
     ('can', 7),
     ('find', 1),
     ('the', 24),
     ('latest', 1),
     ('documentation,', 1),
     ('programming', 1),
     ('guide,', 1),
     ('on', 7),
     ('[project', 1),
     ('web', 1),
     ('page](http://spark.apache.org/documentation.html).', 1),
     ('This', 2),
     ('README', 1),
     ('file', 1),
     ('only', 1),
     ('contains', 1),
     ('basic', 1),
     ('setup', 1),
     ('instructions.', 1),
     ('Building', 1),
     ('built', 1),
     ('using', 5),
     ('[Apache', 1),
     ('Maven](http://maven.apache.org/).', 1),
     ('To', 2),
     ('build', 4),
     ('its', 1),
     ('example', 3),
     ('programs,', 1),
     ('run:', 1),
     ('build/mvn', 1),
     ('-DskipTests', 1),
     ('clean', 1),
     ('package', 1),
     ('(You', 1),
     ('do', 2),
     ('not', 1),
     ('need', 1),
     ('to', 17),
     ('this', 1),
     ('if', 4),
     ('you', 4),
     ('downloaded', 1),
     ('pre-built', 1),
     ('package.)', 1),
     ('more', 1),
     ('than', 1),
     ('one', 3),
     ('thread', 1),
     ('by', 1),
     ('-T', 1),
     ('option', 1),
     ('with', 4),
     ('Maven,', 1),
     ('see', 4),
     ('["Parallel', 1),
     ('builds', 1),
     ('Maven', 1),
     ('3"](https://cwiki.apache.org/confluence/display/MAVEN/Parallel+builds+in+Maven+3).',
      1),
     ('More', 1),
     ('detailed', 2),
     ('documentation', 3),
     ('available', 1),
     ('from', 1),
     ('project', 1),
     ('site,', 1),
     ('at', 2),
     ('["Building', 1),
     ('Spark"](http://spark.apache.org/docs/latest/building-spark.html).', 1),
     ('For', 3),
     ('development', 1),
     ('tips,', 1),
     ('info', 1),
     ('developing', 1),
     ('IDE,', 1),
     ('["Useful', 1),
     ('Developer', 1),
     ('Tools"](http://spark.apache.org/developer-tools.html).', 1),
     ('Interactive', 2),
     ('Scala', 2),
     ('Shell', 2),
     ('The', 1),
     ('easiest', 1),
     ('way', 1),
     ('start', 1),
     ('through', 1),
     ('shell:', 2),
     ('./bin/spark-shell', 1),
     ('Try', 1),
     ('following', 2),
     ('command,', 2),
     ('which', 2),
     ('should', 2),
     ('return', 2),
     ('1000:', 2),
     ('scala>', 1),
     ('sc.parallelize(1', 1),
     ('1000).count()', 1),
     ('Python', 2),
     ('Alternatively,', 1),
     ('prefer', 1),
     ('use', 3),
     ('./bin/pyspark', 1),
     ('And', 1),
     ('run', 7),
     ('>>>', 1),
     ('sc.parallelize(range(1000)).count()', 1),
     ('Example', 1),
     ('Programs', 1),
     ('comes', 1),
     ('several', 1),
     ('sample', 1),
     ('programs', 2),
     ('`examples`', 2),
     ('directory.', 1),
     ('them,', 1),
     ('`./bin/run-example', 1),
     ('<class>', 1),
     ('[params]`.', 1),
     ('example:', 1),
     ('./bin/run-example', 2),
     ('SparkPi', 2),
     ('will', 1),
     ('Pi', 1),
     ('locally.', 1),
     ('MASTER', 1),
     ('environment', 1),
     ('variable', 1),
     ('when', 1),
     ('running', 1),
     ('examples', 2),
     ('submit', 1),
     ('cluster.', 1),
     ('be', 2),
     ('mesos://', 1),
     ('or', 3),
     ('spark://', 1),
     ('URL,', 1),
     ('"yarn"', 1),
     ('YARN,', 1),
     ('"local"', 1),
     ('locally', 2),
     ('thread,', 1),
     ('"local[N]"', 1),
     ('N', 1),
     ('threads.', 1),
     ('abbreviated', 1),
     ('class', 2),
     ('name', 1),
     ('package.', 1),
     ('instance:', 1),
     ('MASTER=spark://host:7077', 1),
     ('Many', 1),
     ('print', 1),
     ('usage', 1),
     ('help', 1),
     ('no', 1),
     ('params', 1),
     ('are', 1),
     ('given.', 1),
     ('Running', 1),
     ('Tests', 1),
     ('Testing', 1),
     ('first', 1),
     ('requires', 1),
     ('[building', 1),
     ('Spark](#building-spark).', 1),
     ('Once', 1),
     ('built,', 1),
     ('tests', 2),
     ('using:', 1),
     ('./dev/run-tests', 1),
     ('Please', 4),
     ('guidance', 2),
     ('how', 3),
     ('[run', 1),
     ('module,', 1),
     ('individual', 1),
     ('tests](http://spark.apache.org/developer-tools.html#individual-tests).', 1),
     ('There', 1),
     ('Kubernetes', 1),
     ('integration', 1),
     ('test,', 1),
     ('resource-managers/kubernetes/integration-tests/README.md', 1),
     ('A', 1),
     ('Note', 1),
     ('About', 1),
     ('Hadoop', 3),
     ('Versions', 1),
     ('uses', 1),
     ('core', 1),
     ('library', 1),
     ('talk', 1),
     ('HDFS', 1),
     ('other', 1),
     ('Hadoop-supported', 1),
     ('storage', 1),
     ('systems.', 1),
     ('Because', 1),
     ('protocols', 1),
     ('have', 1),
     ('changed', 1),
     ('different', 1),
     ('versions', 1),
     ('Hadoop,', 2),
     ('must', 1),
     ('against', 1),
     ('same', 1),
     ('version', 1),
     ('your', 1),
     ('runs.', 1),
     ('refer', 2),
     ('["Specifying', 1),
     ('Version', 1),
     ('Enabling', 1),
     ('YARN"](http://spark.apache.org/docs/latest/building-spark.html#specifying-the-hadoop-version-and-enabling-yarn)',
      1),
     ('building', 2),
     ('particular', 2),
     ('distribution', 1),
     ('Hive', 2),
     ('Thriftserver', 1),
     ('distributions.', 1),
     ('Configuration', 1),
     ('[Configuration', 1),
     ('Guide](http://spark.apache.org/docs/latest/configuration.html)', 1),
     ('online', 1),
     ('overview', 1),
     ('configure', 1),
     ('Spark.', 1),
     ('Contributing', 1),
     ('review', 1),
     ('[Contribution', 1),
     ('guide](http://spark.apache.org/contributing.html)', 1),
     ('information', 1),
     ('get', 1),
     ('started', 1),
     ('contributing', 1),
     ('project.', 1)]



编写独立应用程序执行词频统计


```python
wordCount.foreach(print)
```
