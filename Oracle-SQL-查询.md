---
title: Oracle SQL 查询
date: 2017-12-27 21:17:45
tags:
    - Oracle
---

# 基本查询

```
select
where
distinct
group by
having
order by
```

`order by`和`distinct`一起使用时，`order by` 子句所指定的排列，必须出现在`select`表达式中。

# 子查询

```
select *
from employees
where employee_id in (
    select employee_id
    from salary
    );
```


```
create table tmp_user_objects
as
select *
from tmp_user_objects
where 1 <> 1;
```

```
insert into tmp_user_objects
select *
from user_objects
where object_type = 'TABLE';
```
# 联合语句

- `union`
- `union all`: 并不剔除重复数据
- `intersect`
- `minus`

# 连接

## 自然连接

```
select *
from employees
natural join salary; -- 两表都含有 employee_id 列
```

1. 自然连接必须使用同名列
2. 所有同名列都将作为搜寻条件

## 内连接

```
select e.employee_id, e.employee_name, s.month, s.salary
from employees as e
join salary s
on e.employee_id = s.employee_id;
```

## 外连接

```
select e.employee_id, e.employee_name, s.month, s.salary
from employees e
left join salary s
on e.employee_id = s.employee_id;

select e.employee_id, e.employee_name, s.month, s.salary
from employees e, salary s
where e.employee_id = s.employee_id(+); -- salary 为附属表
```

## 完全连接

```
select e.employee_id, e.employee_name, s.month, s.salary
from employees e
full join salary s
on e.employee_id = s.employee_id;
```

# 层次化查询

```
select col1, col2
from table
start with condition
connect by condition;
```

```
select marker_id, marker_name
from marker
start with marker_name = '亚洲'
connect by prior marker_id = parent_market_id; -- prior 指前一条记录
```

`sys_connect_by_path(列名, 分隔符)`
