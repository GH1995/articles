---
title: quick sort
categories:
  - algorithm
date: 2019-08-01 21:00:39
tags:
---

## quick partition

```cpp
int partition(int a[], int lo, int hi) {
  int pv = a[lo], i = lo, j = hi;
  while (true) {
    while (j >= lo && a[j] >= pv) --j;
    while (i <= hi && a[i] <= pv) ++i;

    // TODO
    if (i >= j) break;
    swap(a[i], a[j]);
    ++i, --j;
  }

  swap(a[lo], a[j]);

  return j;
}
```


## knuth shuffle

```cpp
void shuffle(int a[]){
    for (int i = 0; i < a.length; ++i)
        swap(k, random(0, k));
}
```

## quick sort

```cpp
void qsort(int a[], int lo, int hi) {
  if (lo >= hi) return;
  int p = partition(a, lo, hi);

  qsort(a, lo, p - 1);
  qsort(a, p + 1, hi);
}
```

## quick select 

```cpp
int findKth(int a[], int k) {
  shuffle(a);
  int lo = 0, hi = a.lenght - 1;

  while (lo < hi) {
    int p = partition(a, lo, hi);
    if (p == k)
      return a[k];
    else if (p < k)
      lo = p + 1;
    else
      hi = p - 1;
  }
}
```
