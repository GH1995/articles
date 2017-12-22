---
title: Numpy Note 1
date: 2017-12-01 22:27:37
tags:
- Python
- Numpy
---

Numpy is the fundamental package for scientific computing with Python. It contains among other things:

- a powerful N-dimensional array object
- sophisticated (broadcasting) functions
- tools for interating C/C++ and Fortran code
- useful linear algebra, Fourier transform, and random number capabilities


```
array(ndarray)
	- ndim
	- shape
	- dtype
	- reshape()

	- ones
	- zeros
	- empty
	- eye
```


```
arrage

	arr.astype(np.float64)
	arr[5:8].copt()

	axis 0 -- row
	axis 1 -- col
```

```
bool indexing

	names = np.array(['Bob', 'Joe', 'Will', 'Bob', 'Will', 'Joe', 'Joe'])
	names == 'Bob'
	data[names=='Bob']
```

```
fancy indexing

	arr[[4, 3, 0, 6]]

	CAUTIOUS:	select(arr[][], arr[][], arr[][])

	arr[np.ix_([1, 5, 7, 2], [0, 3, 1, 2])]
	arr[[1, 5, 7, 2]][:, [0, 3, 1, 2]]
```

```
transpose

	- arr.T
	- arr.transpose((1, 0, 2))
	- arr.swapaxes(1, 2)
```

```
ufunc

	- np.sqrt(arr)
	- sqrt
	- exp
	- abs

	- np.maximum(x, y)
	- np.modf(arr)
```

```
array deal data

	np.meshgrid()
	points = np.arrange(-5, 5, 1)
	xs, ys = np.meshgrid(points, points)
	z = np.sqrt(xs**2, + ys**2)
	plt.imshow(z, cmap=plt.cm.gray);plt.colorbar()
```

```
conditional logic as array operation

	xarr = np.array([1.1, 1.2, 1.3, 1.4, 1.5])
	yarr = np.array([2.1, 2.2, 2.3, 2.4, 2.5])
	cond = np.array([True, False, True, True, False])

	result = [(x if c else y)
				for x, y, z in zip(xarr, yarr, cond)]
	result = np.where([cond, arr, yarr])

	arr = randn(4, 4)
	np.where(arr > 0, 2, -2)
	np.where(arr > 0, 2, arr)
```

```
statistical methods

	arr.mean()
	np.mean(arr)
	arr.mean(axis = 1)

	arr.sum(0)
	arr.cumsum(0)
	arr.cumprod(0)
```

```
boolean array's methods

	(arr > 0).sum

	bools.any()
	bools.all()
```

```
sort

	arr.sort()
	arr.sort(1)
```

```
unique

	np.unique(names)
	np.in1d(values, [2, 3, 6])
```

```
save the array in binary format

	np.save('some_array', arr)
	np.savez('array_archive.npz', a = arr, b = arr)
	np.load('some_array.npy')

	np.loadtxt('array_ex.txt', delimiter = ',')
```

```
linear algebra

	x.dot(y)
	np.dot(x, y)

	mat = x.T.dot(y)
	inv(mat)
	q, r = qt(mat)
```

```
random number

	np.random

	samples = np.random.normal(size = (4, 4))
```
