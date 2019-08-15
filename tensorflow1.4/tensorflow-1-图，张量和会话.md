---
title: 'tensorflow(1): 图，张量和会话'
tags:
  - tensorflow
categories:
  - tensorflow1.4
---

## 定义两个不同的图

除了使用默认的计算图， TensorFlow 支持通过 `tf.Graph` 函数来生成新的计算图


```python
import tensorflow as tf
```


```python
g1 = tf.Graph()
with g1.as_default():
    v = tf.get_variable("v", [1],
                        initializer=tf.zeros_initializer())  # 设置初始值为0

g2 = tf.Graph()
with g2.as_default():
    v = tf.get_variable("v", [1], initializer=tf.ones_initializer())  # 设置初始值为1
```


```python
with tf.Session(graph=g1) as sess:
    tf.global_variables_initializer().run()
    with tf.variable_scope("", reuse=True):
        print(sess.run(tf.get_variable("v")))

with tf.Session(graph=g2) as sess:
    tf.global_variables_initializer().run()
    with tf.variable_scope("", reuse=True):
        print(sess.run(tf.get_variable("v")))
```

    [0.]
    [1.]


计算图可以通过 `tf.Graph.device` 函数来指定运行计算的设备。

在一个计算图中，可以通过集合( collection ）来管理不同类别的资 源。

比如通过 `tf.add_to_collection` 函数可以将资源加入一个或多个集合中，然后通过 `tf.get_collection` 获取一个集合里面的所有资源。

这里的资源可以是张量、变量或者运行 TensorFlow 程序所需要的队列资源， 等等。

- `tf.GraphKeys.VARIABLES`
- `tf.GraphKeys.TRAINABLE_VARIABLES`
- `tf.GraphKeys.SUMMARIES`
- `tf.GraphKeys.QUEUE_RUNNERS`
- `tf.GraphKeys.MOVING_AVERAGE_VARIABLES`

##  张量的概念

在张量中并没有 真正保存数字，它保存的是如何得到这些数字的计算过程。


```python
import tensorflow as tf
a = tf.constant([1.0, 2.0], name="a")
b = tf.constant([2.0, 3.0], name="b")
result = a + b
print(result)

sess = tf.InteractiveSession()
print(result.eval())
sess.close()
```

    Tensor("add:0", shape=(2,), dtype=float32)
    [3. 5.]


1. 对中间计算结果的引用
2. `result.get_shape()`

##  会话的使用

会话拥有并管理 TensorFlow 程序运行时的所有资源。

### 创建和关闭会话


```python
# 创建一个会话。
sess = tf.Session()

# 使用会话得到之前计算的结果。
print(sess.run(result))

# 关闭会话使得本次运行中使用到的资源可以被释放。
sess.close()
```

    [3. 5.]


张量的三个属性


- name
- shape
- type

### 使用 with statement 来创建会话


```python
with tf.Session() as sess:
    print(sess.run(result))
```

    [3. 5.]


###  指定默认会话


```python
sess = tf.Session()
with sess.as_default():
    print(result.eval())
```

    [3. 5.]



```python
sess = tf.Session()

# 下面的两个命令有相同的功能。
print(sess.run(result))
print(result.eval(session=sess))
sess.close()
```

    [3. 5.]
    [3. 5.]


TensorFlow 不会自动生成默认的会话，而是需要手动指定。 当默认的会话被指定之后可以通过 `tf.Tensor.eval` 函数来计算 一个张量的取值。

## 使用 tf.InteractiveSession 构建会话

省去将产生的会话注册为默认会话的过程


```python
sess = tf.InteractiveSession ()
print(result.eval())
sess.close()
```

    [3. 5.]


## 通过ConfigProto配置会话

配置两个参数即可


```python
config = tf.ConfigProto(allow_soft_placement=True, #  允许GPU 上的运算可以放到 CPU 上进行
                        log_device_placement=True  #  为 True 时日志中将会记录每个节点被安排在哪个设备上以方便调试
                       )
```


```python
sess1 = tf.InteractiveSession(config=config)
sess2 = tf.Session(config=config)
```
