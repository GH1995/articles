---
title: MySQL Cheat Sheet
date: 2017-12-19 21:05:36
tags:
    - MySQL
---

## Create/Delete Database

```
CREATE DATABASE mabase;
CREATE DATABASE mabase CHARACTER SET utf8;
ALTER DATABASE mabase CHARACTER SET utf8;
```

## Browsing

```
SHOW DATABASE;
SHOW TABLES;
SHOW FIELDS FROM table = DESCRIBE table;
SHOW PROCESSLIST;
KILL precess_number;
```

## Select- Join

```
SELECT ... FROM t1 JOIN t2 ON t1.id1 = t2.di2 WHERE condition;
SELECT ... FROM t1 LEFT JOIN t2 ON t1.id1 = t2.id2 WHERE condition;
SELECT ... FROM t1 JOIN (t2 JOIN t3 ON ..) ON ..;
```

## Insert

```
INSERT INTO table1 (field1, field2, ...) VALUES(value1, value2, ...);
```

## Delete

```
```
