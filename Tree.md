---
title: Tree
date: 2017-12-21 14:16:21
tags:
    - Algorithm
---

# Binary Tree
## core
```c
typedef struct BiTNode {
    int data;
    struct BiTNode *lchild, *rchild;
} BiTNode, *BiTree;
```

## methods

```c
void PreOrder(BiTree b);
void InOrder(BiTree b);
void PostOrder(BiTree b);
void LevelOrder(BiTree b);
```

---

# Thread Binary Tree

## core

```c
enum PointerTag {Link, Thread};
typedef struct BiThrNode {
    int data;
    struct BiThrNode *lchild, *rchild;
    PointerTag LTag, RTag;
};
```
