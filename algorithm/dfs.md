---
title: dfs
date: 2019-08-02 00:06:38
tags:
---


```cpp
struct GraphNode {
  int val;
  vector<GraphNode *> neighbors;
};

unordered_set<GraphNode *> visited;
```

### Recursive DFS

```cpp
void dfs(GraphNode &nd) {
  printf("%d", nd.val);
  visited.insert(&nd);

  for (auto next : nd.neighbors)
    if (!visited.count(next)) {
      dfs(*next);
    }
}
```

### Non-recursive DFS

```cpp
void no_recursive_dfs(GraphNode &start) {
  stack<GraphNode *> s;
  s.push(&start);

  while (!s.empty()) {
    auto cur = s.top();
    s.pop();
    printf("%d\n", cur->val);

    for (auto next : cur->neighbors) {
      if (!visited.count(next)) {
        s.push(next);
        visited.insert(next);
      }
    }
  }
}
```
