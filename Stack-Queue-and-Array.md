---
title: 'Stack, Queue and Array'
date: 2017-12-21 14:07:06
tags:
    - data structure
---

```cpp
struct SqStack {
    int elem[MAX];
    int top;
};

Push: elem[++top] = e;
Pop : e = elem[top--];
```

```cpp
struct SqStack {
    int *base;
    int *top;
    int stacksize;
};

Push: *top++ = e;
Pop : e = *--top;
```

---

```cpp
struct SeQueue {
    int data[MAX];
    int rear, front;
};

EnQueue: sq-data[++sq->rear] = x;
DeQueue: x = sq->data[++sq->front];
```
