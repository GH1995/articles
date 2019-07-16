---
title: Oracle 数据库与数据表
tags:
  - Oracle
categories:
  - oracle
date: 2017-12-27 20:36:19
---

# 配置/管理 Oracle 数据库

## sqlplus

1. 利用 sqlplus 登录数据库

```sql
sqlplus username/password@ netservicename
```

2. 查看数据库参数

```sql
show parameter instance_name;
```

3. 关闭/启动数据库

```sql
sqlplus /@tst as sysdba;
shutdown immediate;
startup;
```

4. 修改系统参数

```sql
show parameter recovery;
alter system set db_recovery_file_dest_size=5 scope=both;
```

# 表空间

## 创建表空间

```sql
create tablespace test
datefile 'E:\database\data.dbf'
size 20M
autoextend on next 5M -- optional
maxsize 500M; -- optional
```

`test` :表空间名称

```sql
select tablespace, file_name
from dab_data_file
order by file_name;
```

## 表空间的使用

|      SYSTEM     |   USERS  |
|:---------------:|:--------:|
| `sys`和`system` | 普通用户 |

利用`alter database`修改数据库的默认表空间

```sql
alter database
default tablespace test;
```

## 表空间的重命名及删除

```sql
alter tablespace test
rename to test_data;
```

```sql
drop tablespace test_data
including contents and datafiles;
```

# Oracle 数据表

## 创建 Oracle 数据表

```sql
create table 表名 (
    列      数据类型,
...
) tablespace 表空间;
```

```sql
describe student;
```

## 数据表的相关操作

```sql
-- 增加列
alter table student
add (class_id number);

-- 修改数据类型
alter table student
modify (class_id varchar2(20));

-- 删除列
alter table student
drop column class_id;

alter table student
rename column student_id to id;
```

将表`student`转移到表空间`users`中

```sql
alter table student
move tablespace users;
```

## 删除数据表

```sql
drop table student;
```

# 特殊的数据表`dual`

```sql
select sysdata
from dual;


select 5*4+7 result
from dual;
```
