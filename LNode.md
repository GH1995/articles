---
title: LNode
date: 2017-12-21 13:47:08
tags:
    - data structure
---

# core
```c
typedef struct LNode {
    int data;
    struct LNode *next;
}LNode, *LinkList;
```

# methods

```c
LinkList CreateListFirst(LinkList &L, int a[], int n);
LinkList CreateListLast(LinkList &L, int a[], int n);

int LengthList(LinkList L);
LNode *GetLinkList(LinkList L, int i);
LNode *LocateLinkList(LinkList L, int x);
```

---

```c
LinkList CreateListFirst(LinkList &L, int a[], int n)
{
    L = (LinkList)malloc(sizeof(LNode));
    L->next = NULL;

    for (int i = 0; i < n; i++) {
        LNode *s = (LNode *s)malloc(sizeof(LNode));
        s->data = a[i];
        s->next = L->next;
        L->next = s;
    }
}
```

```c
LinkList CreateListLast(LinkList &L, int a[], int n)
{
    L = (LinkList)malloc(sizeof(LNode));
    L->next = NULL;
    LNode *r = L;

    for (int i = 0; i < n; i++) {
        s = (LNode *)malloc(sizeof(LNode));
        s->data = a[i];
        r->next = s;
        r = s;
    }
    r->next = NULL;
}
```

```c
int LengthList(LinkList L)
{
    LNode *p = L;
    int j = 0;

    while (p->next) {
        p = p->next;
        j++;
    }
    return j;
}
```

```c
LNode *GetLinkList(LinkList L, int i);
    while (p->next != NULL && j < i)

LNode *LocateLinkList(LinkList L, int x)
    p = L->next;
    while (p != NULL && p->data != x)
```

```c
Insert *s after *p
    s->next = p->next;
    p->next = s;

Insert *s before *p
    q = L;
    while (q->next != p)
        q = q->next;
    s->next = q->next;
    q->next = s;

delete *p
    q->next = p->next;
    free(p);
```
