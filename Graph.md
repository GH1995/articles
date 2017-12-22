---
title: Graph
date: 2017-12-21 14:22:44
tags:
    - data structure
---

```c
struct MGraph {
    int vexs[MAX];
    int edges[MAX][MAX];

    int vexnum, arcnum;
};
```


```c
struct ArchNode {
    int adjvex;
    struct ArchNode *nextarc;
};

typdef struct VNode {
    int data;
    ArchNode *firstarc;
}VNode, AdjList[MAX];

struct ALGraph {
    AdjList adjlist;
    int vexnum, arcnum;
};
```
