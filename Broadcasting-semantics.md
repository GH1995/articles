---
title: Broadcasting semantics
date: 2017-12-28 20:15:26
tags:
    - Pytorch
    - Python
    - Deep Learning
---

# General semantics

- Each tensor has at least one dimension.
- When iterating over the dimension sizes, starting at the trailing dimension, the dimension size must either be equal, one of them is 1, or one of them does exist.

```python
x = torch.FloatTensor(5, 7, 3)
y = torch.FloatTensor(5, 7, 3)
# same shapes are always broadcastable

x = torch.FloatTensor(5, 3, 4, 1)
y = torch.FloatTensor(   3, 1, 1)
# x and y are broadcastable
# 1st trailing dimension: both have size 1
# 2nd trailing dimension: y has size 1
# 3rd trailing dimension: x size == y size
# 4th trailing dimension: y dimension doesn't exist

# but
x = torch.FloatTensor(5, 2, 4, 1)
y = torch.FloatTensor(   3, 1, 1)
# x and y are not broadcastable, because in the 3rd trailing dimension 2 != 3
```

```python
x = torch.FloatTensor(5, 1, 4, 1)
y = torch.FloatTensor(   3, 1, 1)
(x+y).size()
torch.Size([5, 3, 4, 1])

# but not necesary:
x = torch.FloatTensor(1)
y = torch.FloatTensor(3, 1, 7)
(x+y).size()
torch.Size([3, 1, 7])

x = torch.FloatTensor(5, 2, 4, 1)
y = torch.FloatTensor(3, 1, 1)
(x+y).size()
```
