---
title: Oracle SQL 更新数据
tags:
  - Oracle
categories:
  - oracle
date: 2017-12-27 21:38:05
---

# insert

```
insert into 表名(列名)
values(值);
```

```
insert into 表名(列名)
select 
```

# update

```
update 表名
set 列 = 新值
where ;
```

# delete

```
delete from 表名; -- DML
```

```
truncate table students; -- DDL，无法回滚
```
