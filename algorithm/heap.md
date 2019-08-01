---
title: heap
categories:
  - algorithm
date: 2019-08-01 21:30:12
tags:
---

## logical structure of PQ

## array representation of PQ

## PQ implementation

maxPQ

```cpp
void add(int n); // insert element to PQ --> stay tuned  
int poll(); // get and remove top element --> stay tuned 

void siftup();
void siftdown();
```

### siftup

```cpp
void siftup() {
  int i = size; // i is the index of the newly added element 

  while (i > 1 && pq[i / 2] < pq[i]) {
    swap(pq[i / 2], pq[i]);
    i /= 2;
  }
}
```

### siftdown

```cpp
void siftdown() {
  int i = 1;  // siftdown root node

  while (i * 2 <= size) {    // while the node is not in last level
    int max = pq[i], j = i;  // j is the element to swap

    if (pq[i * 2] > max)  // left
      j = i * 2, max = pq[i * 2];

    if (i * 2 + 1 <= size && pq[i * 2 + 1] > max)  // right
      j = i * 2 + 1, max = pq[i * 2 + 1];

    if (j == i)  // stop when node is bigger than both child
      return;

    swap(pq[i], pq[j]);
    i = j;
  }
}
```

### add


```cpp
void add(int n) {
  pq[++size] = n;
  siftup();
}
```

### poll

```cpp
int poll() {
  int top = pq[1];
  pq[1] = pq[size--];
  siftdown();
  return top;
}
```

## PQ application

### heapsort

把数组的元素`add`到一个`pq`里, 然后依次`poll`出来, 得到的序列就是排序好了的

## heapify

假设一共有h(=lgN)层, 由于最后一层的节点不必调用siftdown, 我们只要从倒数第二层开始调用siftdown即可 

```cpp
void heapify(int a[]){
    int n = a.length;

    for(int i = n/2; i <= 1; --i)
        siftdown(a, i);
}
```

### top $k$ elements of a stream

```
if (minPQ.size() < K)
    minPQ.add(n)
else if (minPQ.size() == K){
    if (n > minPQ.top())
        minPQ.poll(), minPQ.add(n);
}
```

### median of a stream
