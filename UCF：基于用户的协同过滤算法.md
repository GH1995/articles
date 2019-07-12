---
title: UCF：基于用户的协同过滤算法
date: 2019-07-11 16:51:28
tags:
    - rs
---

## 基础算法


1992 年提出UCF，UCF 的两个步骤

1. 找到和目标用户相似的用户集合
2. 找到这个集合中用户喜欢的，且用户未见过的物品推荐

###  如何计算两个用户 $u, v$ 的兴趣相似度？

- $N(u)$ 表示用户 $u$ 曾经有过正反馈的物品集合
- $N(v)$ 表示用户 $v$ 曾经有过正反馈的物品集合

利用 Jaccard 公式(余弦也行)计算用户 $u, v$ 的兴趣相似度 $w_{uv}$

$$
w_{u v}=\frac{|N(u) \cap N(v)|}{|N(u) \cup N(v)|}
$$

**[余弦相似度](https://www.wikiwand.com/en/Cosine_similarity)，重点**

$$
w_{u v}=\frac{|N(u) \cap N(v)|}{\sqrt{|N(u)||N(v)|}}
$$

![用户行为记录举例](https://s2.ax1x.com/2019/07/11/ZRpQCd.png)

用户 $A$ 对物品  $\{a, b, d\}$ 有过正反馈，用户 $B$ 对物品 $\{a, c\}$ 有过正反馈，利用**余弦相似度**计算得出

$$
w_{A B}=\frac{|\{a, b, d\} \cap\{a, c\}|}{\sqrt{\{a, b, d\}| |\{a, c\} |}}=\frac{1}{\sqrt{6}}
$$

同理可以计算出用户 $A$ 和 用户 $C, D$ 的相似度

$$
\begin{aligned} w_{A C} &=\frac{|\{a, b, d\} \cap\{b, e\}|}{\sqrt{|\{a, b, d\}||\{b, e\}|}}=\frac{1}{\sqrt{6}} \\ w_{A D} &=\frac{|\{a, b, d\} \cap\{c, d, e\}|}{\sqrt{|\{a, b, d\}||\{c, d, e\}|}}=\frac{1}{3} \end{aligned}
$$

我们可以撸一遍余弦相似度计算的代码，反正就是两个 `for` 循环

```python
def UserSimilarity(train):
    W = dict()

    for u in train.keys():
        for v in train.keys():
            if u == v:
                continue

            W[u][v] = len(train[u] & train[v])  # 拿出两行 交 一下
            W[u][v] /= math.sqrt(len(train[u]) * len(train[v]) * 1.0) # 这里 N(u) x N(v) 就是 len(u) x len(v)

    return W
```

![用户-物品倒排表](https://s2.ax1x.com/2019/07/11/ZRVk2F.png)

### 如何**快速**计算两个用户 $u, v$ 的兴趣相似度？

**难点**

参考[这里](https://blog.csdn.net/fycghy0803/article/details/79880452)，计算时间复杂度为 $O(|U|^2)$。事实上很多用户并没有对同一物品产生过反馈，即 $|N(u) \cap N(v)|=0$。如果计算这种用户的相似度（肯定为 0），无疑是浪费了时间。怎么办呢？

解决思路是，先计算出$|N(u) \cap N(v)| \neq 0$ 的用户 $pair(u, v)$，然后再对这种情况除以分母 $\sqrt{|N(u)||N(v)|}$。

首先，建立 $item \to user$ 的倒排表，建立物品和反馈用户列表之间的关系。

然后考虑，建立**用户稀疏矩阵**，大小为 用户数 $\times$ 用户数

$$
C[u][v]=|N(u) \cap N(v)|
$$

假设用户 $u$ 和 用户 $v$ 同时反馈过 $K$ 个物品，就有 $C[u][v]=K$。

好了，这里我们扫描刚刚建立好的**倒排表**的用户列表，将这个列表中的两两用户$u, v$对应的 $C[u][v]$ 加 1。最终就可以得到用户之间不为 0 的 $C[u][v]$。

比之，之前的方法，这里我们这里有两点变化：第一，遍历 $item$，**这个长，但是只有一遍**；第二，针对$item \to user$ 的 $user\_list$ 两两$(u, v) +1$，**这个需要遍历两遍，但是很短**。

最后，在第二步我们建立了余弦距离的分子$C[u][v]$，计算的时候，拿出分母 $\sqrt{|N(u)||N(v)|}$ 即可，$N(u)$的定义是用户反馈的 $item$ 集合, 相对容易计算。


改进之后的代码

```python
def UserSimilarity(train):
    # 建立 item -> users 倒排表
    item_users = dict()
    for u, item in train.items():
        for i in item.keys():
            if i not in item_users:
                item_users[i] = set()

            item_users[i].add(u)  # 这里可以用 defaultdict 替换

    # 计算分子项，即 $ C[u][v]=|N(u) \cap N(v)| $
    C = dict()  # 用户 u, v 矩阵
    N = dict()  # 用户 -> 反馈次数

    for i, users in item_users.items():
        for u in users:
            N[u] += 1
            for v in users:
                if u == v:
                    continue
                C[u][v] += 1

    # 计算相似度矩阵 W
    W = dict()
    for u, related_users in C.items():
        for v, cuv in related_users.items():
            W[u][v] = cuv / math.sqrt(N[u]*N[v])

    return W
```

### 如何度量 UCF 算法中，用户 $u$ 对物品 $i$的兴趣程度？

- $S(u, K)$ 和用户 $u$ 兴趣最接近的 $K$ 个用户的集合
- $N(i)$ 对物品 $i$ 有过反馈的用户集合
- $w_{u v}$ 是用户 $u, v$ 的兴趣相似度
- $r_{v i}$ 用户 $v$ 对物品 $i$ 的兴趣，默认为 1

这里计算的方式就是，先找到用户$u$的 Top $K$ 的好基友，问问这些基友有没有买过 $N(i)$（比如 airpods）。基友们对新款 airpods 的评分是 $r_{v i}$，我们再把用户$u$对各个基友$v$的人品评价记作 $w_{uv}$，作为权重。最后一步，把 **权重$\times$ 评价** 就是最后的**评分**。到底买不买，就看这个评分了！

$$
p(u, i)=\sum_{v \in S(u, K) \cap N(i)} w_{u v} r_{v i}
$$

```python
def Recommand(user, train, w):
    rank = dict()
    interacted_items = train[user]

    for v, wuv in sorted(W[u].items, key=itemgetter(1), reverse=True)[0:K]: # 找打了用户 u 的 K 个基友
        for i, rvi in train[v].items: # K 个基友最近看了什么
            if i in interacted_items: # 过滤用户见过的物品，反馈过的
                continue

            rank[i] += wuv*rvi # 上面讲的打分函数，对物品 i 打分

    return rank
```

这里 $K$ 是超参数，评价指标为准确率，召回率，覆盖率，流行度。

- 准确率和召回率对 $K$ 不是很敏感
- $K$越大，流行度上越趋近于全局热门物品
- $K$ 越大，覆盖率越低

## 用户相似度的改进

根据用户行为计算用户的兴趣相似度，**惩罚用户$u$和用户$v$共同兴趣列表中的热门物品**！

$$
w_{t v v}=\frac{\sum_{i \in N(u) \cap N(v)} \frac{1}{\log 1+|N(i)|}}{\sqrt{|N(u)||N(v)|}}
$$

注意这里分子的变化

$$
|N(u) \cap N(v)| \longrightarrow \sum_{i \in N(u) \cap N(v)} \frac{1}{\log 1+|N(i)|}
$$

$\frac{1}{\log 1+|N(i)|}$ 惩罚了每一个共同兴趣，我们把改进后的算法叫做**User-IIFF算法**。

我们再撸一遍代码

```python
def UserSimilarity(train):
    # 建立倒排表
    item_users = dict()

    for u, items in train.items():
        for i in items.keys():
            if i not in item_users:
                item_users[i] = set()

            item_users[i].add(u)

    # 计算分子项
    C = dict()
    N = dict()

    for i, users in item_users.items():
        for u in users:
            N[u] += 1
            for v in users:
                if u == v:
                    continue
                C[u][v] += 1/math.log(1+len(users))

    # 计算相似度矩阵
    W = dict()
    for u, related_users in C.items():
        for v, cuv in related_users.items():
            W[u][v] = cuv/math.sqrt(N[u]*N[v])

    return W
```


实验证明 UserCF-IIF 在各项性能上略优于 UserCF。

参考资料：推荐系统实践，项亮，第二章
