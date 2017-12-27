---
title: Oracle 中的字符型及处理方法
date: 2017-12-27 21:43:55
tags:
    - Oracle
---

# 字符型简介

## 固定长度字符串`char(n)`

利用空格在右端补齐，限长2000

## `varchar(n)`
限长4000

## `varchar2(n)`
推荐

# 字符型分析

`char(n)`不适用与声明变量。

# 字符型处理

向左补全字符串——`lpad`函数

```
lpad(string, padded_length, [pad_string])

lpad('1', 4, '0')
```
向右补全字符串——`lpad`函数

返回字符串的小写形式——`lower()`函数

```
where lower(username) = 'system';
```
返回字符串的大写形式——`upper()`函数

单词首字符大写——`initcap()`函数
返回字符串长度——`length()`函数
截取字符串——`substr()`函数
```
substr(string, start_index, length)
```

- `instr`
- `ltrim`
- `rtrim`
- `trim`
- `concat`
- `translate`
- `reverse`
