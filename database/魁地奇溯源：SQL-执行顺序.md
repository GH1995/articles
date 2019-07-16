---
title: 魁地奇溯源：SQL 执行顺序
tags:
  - SQL
categories:
  - database
date: 2019-07-03 11:04:48
---

执行顺序：

1. `FROM`
2. `ON`
3. `JOIN`
4. `WHERE`
5. `GROUP BY`
6. `WITH {CUBE | ROLLUP}`
7. `HAVING`
8. `SELECT`
9. `DISTINCT`
10. `ORDER BY`
11. `LIMIT`
