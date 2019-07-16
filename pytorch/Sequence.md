---
title: Sequence
tags:
  - Python
categories:
  - pytorch
date: 2017-12-03 13:20:04
---


# Sequence

Sequence
:    iterable objects visited through index

- `__len__()`
- `len()`


# Sequence's basic operation

Sequence objects have a method named `__getitem__(self, key)`, so it can be visited by `s[i]`.

```
b = b'ABCDEFG'
b[0] ==> 65
```

slice

The basic form of slice is `s[i:j]` and `s[i:j:k]`. `slice` object is used to save index information of slice, such as `slice(start, stop, step)`. `slice` object has attributes such as '.start', '.stop' and '.step', method such as 'indice(len)' which return a tuple.

```
s[::-1] 	==> 'fedcba'

b[0:1] 		==> b'A'
b[0:len(b)] ==> b'ABCDEF'

slice_obj = slice(1, len(s), 2)
s[slice_obj]==> 'bdf'

slice_obj.indices(4)
```

```
s1 + s2
s1 += s2
s2 *= 2
```

Judge a object wehther or not exist in sequence `s`.

```
x in s
x not in s
s.count(x)
s.index(value[, start [, stop]])
```

```
sorted(iterable, key = None, reverse = False)
key = str.lower
```

```
len()
max()
min()
sum()
```

```
all(iterable)
any(iterable)
```

sequence unpacking

```
a, b = (1, 2)
sid, name, (chinese, math, english) = data
```

tuple variable `*`
assign a number of variable to a tuple variable

```
first, *middles, last = range(10)
```

temporary variable`_`
obtain partial data

```
_, b, _ = (1, 2, 3)
```


# tuple

define a tuple

- `x1, [x2, ..., xn]`
- `(x1, [x2, ..., xn])`
- `tuple()`
- `tuple(iterable)`
- `(1,)`

# list

define a list

- `[x1, [x2, ..., xn]]`
- `list()`
- `list(iterable)`

basic operations

- `del s[index]`
- `s[i:j] = x`
- `dd s[i:j]`, equal as `s[i:j] = []`
- `s[i:j] = []`

list's methods

- `s.append(x)`
- `s.clear()`
- `s.copy()`
- `s.extend(t)`
- `s.insert(i, x)`
- `s.pop()`
- `s.remove(x)`
- `s.reverse()`
- `s.sort()`

list comprehension

```
[i**2 for i in range(10)]
[i for i in range(10) if i%2 == 0]
[(x, y, x*y) for x in range(1, 4) for y in range(1, 4) if x>= y]
```

# string

```
ord('A')	==>	unicode
chr(65)		==> char
```


```
str.strip()
str.lstrip()
str.rstrip()
str.zfill(width)
str.center()
str.ljust()
str.rjust()
str.expandtabs()
```

```
test, find and replace

str.startwith()
str.endwith()
str.count()
str.index()
str.rindex()
str.find()
str.rfind()
str.replace()
```

```
str.split()			==>	list
str.rsplit()
str.partition(seq)	==>	(left, seq, right)
str.rpartition(seq)
str.splitlines()	==> list
str.join()			==> string
```

```
str.maketrans()
str.translate()
```


```
b.decode(encoding, errors)
s.encode(encoding = 'utf-8', errors = 'strict')
```

NOT SUGGESTING

```
string % (values)
```

```
'%(lang)s has %(num)03d quote types.' % {'lang':Python, 'num':2}
```


# bytes and bytearray


- bytes
- bytearray
- memoryview

```
b = s.encode()
```
