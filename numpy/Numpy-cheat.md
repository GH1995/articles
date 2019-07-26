---
title: Numpy cheat
tags:
  - numpy
categories:
  - numpy
date: 2019-07-24 01:30:43
---

```python
import numpy as np
```

axis = 0 是从左往右看

axis = 1 是从上往下看

## 创建 Arrays

三种办法

1. array函数：接受一切序列型的对象(`list`/`set`/`tuple`/`ndarray`)
2. zeros/ones/empty函数，传入一个表示形状的元组(tuple)
3. linspace函数/arange函数


```python
a = np.array([1, 2, 3])
a
```




    array([1, 2, 3])




```python
b = np.array([(1.5, 2, 3), (4, 5, 6)], dtype=float)
b
```




    array([[1.5, 2. , 3. ],
           [4. , 5. , 6. ]])




```python
c = np.array([[(1.5, 2, 3), (4, 5, 6)], [(3, 2, 1), (4, 5, 6)]], dtype=float)
c
```




    array([[[1.5, 2. , 3. ],
            [4. , 5. , 6. ]],
    
           [[3. , 2. , 1. ],
            [4. , 5. , 6. ]]])



**另外两种创建方式**


```python
np.zeros((3, 4))
```




    array([[0., 0., 0., 0.],
           [0., 0., 0., 0.],
           [0., 0., 0., 0.]])




```python
np.ones((2, 3, 4), dtype=np.int16)
```




    array([[[1, 1, 1, 1],
            [1, 1, 1, 1],
            [1, 1, 1, 1]],
    
           [[1, 1, 1, 1],
            [1, 1, 1, 1],
            [1, 1, 1, 1]]], dtype=int16)




```python
d = np.arange(10, 25, 5)
d
```




    array([10, 15, 20])




```python
np.linspace(0, 2, 9)
```




    array([0.  , 0.25, 0.5 , 0.75, 1.  , 1.25, 1.5 , 1.75, 2.  ])




```python
e = np.full((2, 2), 7)
e
```




    array([[7, 7],
           [7, 7]])




```python
f = np.eye(2)
f
```




    array([[1., 0.],
           [0., 1.]])




```python
np.random.random((2, 2))
```




    array([[0.15483956, 0.38346647],
           [0.13780649, 0.21760297]])




```python
np.empty((3, 2))
```




    array([[1.5, 4. ],
           [2. , 5. ],
           [3. , 6. ]])



## IO

### 保存二进制文件


```python
np.save('my_array', a)
np.savez('my_array', a, b)

np.load('my_array.npy')
```




    array([1, 2, 3])



### 保存文本文件


```python
np.savetxt("myarray.csv", a, delimiter=',')

np.loadtxt('myarray.csv')
np.genfromtxt('myarray.csv', delimiter=',')
```




    array([1., 2., 3.])



## data types


```python
np.int64, np.float32, np.complex, np.bool, np.object, np.string_, np.unicode_
```




    (numpy.int64, numpy.float32, complex, bool, object, numpy.bytes_, numpy.str_)



**查看arr属性**


```python
a.shape, len(a), b.ndim, e.size, b.dtype, b.dtype.name, b.astype(int)
```




    ((3,), 3, 2, 4, dtype('float64'), 'float64', array([[1, 2, 3],
            [4, 5, 6]]))



## 数学操作


```python
print("a = ", a)
print("b = ", b)
```

    a =  [1 2 3]
    b =  [[1.5 2.  3. ]
     [4.  5.  6. ]]



```python
g = a - b
g
```




    array([[-0.5,  0. ,  0. ],
           [-3. , -3. , -3. ]])




```python
np.subtract(a, b)
```




    array([[-0.5,  0. ,  0. ],
           [-3. , -3. , -3. ]])




```python
b + a
```




    array([[2.5, 4. , 6. ],
           [5. , 7. , 9. ]])




```python
np.add(b, a)
```




    array([[2.5, 4. , 6. ],
           [5. , 7. , 9. ]])




```python
a / b
```




    array([[0.66666667, 1.        , 1.        ],
           [0.25      , 0.4       , 0.5       ]])




```python
np.divide(a, b)
```




    array([[0.66666667, 1.        , 1.        ],
           [0.25      , 0.4       , 0.5       ]])




```python
a * b
```




    array([[ 1.5,  4. ,  9. ],
           [ 4. , 10. , 18. ]])




```python
np.multiply(a, b)
```




    array([[ 1.5,  4. ,  9. ],
           [ 4. , 10. , 18. ]])




```python
np.exp(b)
```




    array([[  4.48168907,   7.3890561 ,  20.08553692],
           [ 54.59815003, 148.4131591 , 403.42879349]])




```python
np.sqrt(b)
```




    array([[1.22474487, 1.41421356, 1.73205081],
           [2.        , 2.23606798, 2.44948974]])




```python
np.sin(a)
```




    array([0.84147098, 0.90929743, 0.14112001])




```python
np.cos(b)
```




    array([[ 0.0707372 , -0.41614684, -0.9899925 ],
           [-0.65364362,  0.28366219,  0.96017029]])




```python
np.log(a)
```




    array([0.        , 0.69314718, 1.09861229])




```python
print("e = ", e)
print("f = ", f)
```

    e =  [[7 7]
     [7 7]]
    f =  [[1. 0.]
     [0. 1.]]



```python
e.dot(f) # 这是 Hadamard积
```




    array([[7., 7.],
           [7., 7.]])



## 比较


```python
a == b # 元素级别的比较
```




    array([[False,  True,  True],
           [False, False, False]])




```python
a < 2
```




    array([ True, False, False])




```python
np.array_equal(a, b) # 数组级别的比较
```




    False



## 聚集函数


```python
a
```




    array([1, 2, 3])




```python
a.sum()
```




    6




```python
a.min()
```




    1




```python
b
```




    array([[1.5, 2. , 3. ],
           [4. , 5. , 6. ]])




```python
b.max(axis=0)
```




    array([4., 5., 6.])




```python
b.cumsum(axis=1)
```




    array([[ 1.5,  3.5,  6.5],
           [ 4. ,  9. , 15. ]])




```python
a.mean()
```




    2.0




```python
np.std(b)
```




    1.5920810978785667



## 复制arr


```python
h = a.view()
```


```python
np.copy(a)
```




    array([1, 2, 3])




```python
h = a.copy() # 深拷贝
```

## 排序 arr


```python
a.sort()
a
```




    array([1, 2, 3])




```python
c 
```




    array([[[1.5, 2. , 3. ],
            [4. , 5. , 6. ]],
    
           [[3. , 2. , 1. ],
            [4. , 5. , 6. ]]])




```python
c.sort(axis=0)
c
```




    array([[[1.5, 2. , 1. ],
            [4. , 5. , 6. ]],
    
           [[3. , 2. , 3. ],
            [4. , 5. , 6. ]]])



## subsetting, slicing, indexing

### 一个`[]`就是子集


```python
a[2]
```




    3




```python
b[1, 2]
```




    6.0



### 一个`[:]`就是切片


```python
a[0:2]
```




    array([1, 2])




```python
b[0:2, 1]
```




    array([2., 5.])




```python
b[:1]
```




    array([[1.5, 2. , 3. ]])




```python
c[1, ...]
```




    array([[3., 2., 3.],
           [4., 5., 6.]])




```python
a[::-1]
```




    array([3, 2, 1])



### 其他乱七八糟的就是index，包括`[[]]`和`<`


```python
a[a < 2]
```




    array([1])




```python
b[[1, 0, 1, 0], [0, 1, 2, 0]]
```




    array([4. , 2. , 6. , 1.5])




```python
b[[1, 0, 1, 0]][:, [0, 1, 2, 0]]
```




    array([[4. , 5. , 6. , 4. ],
           [1.5, 2. , 3. , 1.5],
           [4. , 5. , 6. , 4. ],
           [1.5, 2. , 3. , 1.5]])



## Array 操作

### 转置


```python
b 
```




    array([[1.5, 2. , 3. ],
           [4. , 5. , 6. ]])




```python
i = np.transpose(b)
i
```




    array([[1.5, 4. ],
           [2. , 5. ],
           [3. , 6. ]])




```python
i.T
```




    array([[1.5, 2. , 3. ],
           [4. , 5. , 6. ]])



### 改变 shape


```python
b.ravel()
```




    array([1.5, 2. , 3. , 4. , 5. , 6. ])




```python
print("g = ", g)
g.reshape(3, -2)
```

    g =  [[-0.5  0.   0. ]
     [-3.  -3.  -3. ]]





    array([[-0.5,  0. ],
           [ 0. , -3. ],
           [-3. , -3. ]])



### 增删元素


```python
np.append(h, g)
```




    array([None, -0.5, 0.0, 0.0, -3.0, -3.0, -3.0], dtype=object)




```python
np.insert(a, 1, 5)
```




    array([1, 5, 2, 3])




```python
np.delete(a, [1])
```




    array([1, 3])



### 连接


```python
np.concatenate((a, d), axis=0)
```




    array([ 1,  2,  3, 10, 15, 20])




```python
print('a = ', a)
print('b = ', b)
np.vstack((a, b))
```

    a =  [1 2 3]
    b =  [[1.5 2.  3. ]
     [4.  5.  6. ]]





    array([[1. , 2. , 3. ],
           [1.5, 2. , 3. ],
           [4. , 5. , 6. ]])



和上面的功能一样


```python
print('e = ', e)
print('f = ', f) 
np.r_[e, f]
```

    e =  [[7 7]
     [7 7]]
    f =  [[1. 0.]
     [0. 1.]]





    array([[7., 7.],
           [7., 7.],
           [1., 0.],
           [0., 1.]])




```python
np.hstack((e, f))
```




    array([[7., 7., 1., 0.],
           [7., 7., 0., 1.]])




```python
np.column_stack((a, d))
```




    array([[ 1, 10],
           [ 2, 15],
           [ 3, 20]])



### 划分arr


```python
np.hsplit(a, 3)
```




    [array([1]), array([2]), array([3])]




```python
np.vsplit(c, 2)
```




    [array([[[1.5, 2. , 1. ],
             [4. , 5. , 6. ]]]), array([[[3., 2., 3.],
             [4., 5., 6.]]])]


