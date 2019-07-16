---
title: MySQL创建用户与授权
tags:
  - MySQL
categories:
  - linux
date: 2018-06-18 22:00:09
---

## 创建用户

```SQL
CREATE USER 'usernaem'@'host' IDENTIFIED BY 'passwd';

% host：指定该用户在哪个主机上可以登陆，如果是本地用户可用localhost，如果想让该用户可以从任意远程主机登陆，可以使用通配符%
```


## 授权

```SQL
GRANT privileges ON databasename.tablename TO 'username'@'host';

GRANT ALL ON *.* TO 'pig'@'%';
```

- `privileges`：用户的操作权限，如`SELECT`，`INSERT`，`UPDATE`等，如果要授予所的权限则使用`ALL`
- `tablename`：表名，如果要授予该用户对所有数据库和表的相应操作权限则可用`*`表示，如`*.*`


用以上命令授权的用户不能给其它用户授权，如果想让该用户可以授权，用以下命令:

```SQL
GRANT privileges ON databasename.tablename TO 'username'@'host' WITH GRANT OPTION;
```

## 设置与更改用户密码

```SQL
SET PASSWORD FOR 'username'@'host' = PASSWORD('newpassword');
```

## 如果是当前登陆用户用:

```SQL
SET PASSWORD = PASSWORD("newpassword");
```

## 撤销用户权限

```SQL
REVOKE privilege ON databasename.tablename FROM 'username'@'host';

REVOKE SELECT ON *.* FROM 'pig'@'%';
```

## 删除用户

```SQL
DROP USER 'username'@'host';
```

## 总结
```SQL
SHOW GRANTS FOR 'pig'@'%';
```

**参考：**
1. https://www.jianshu.com/p/d7b9c468f20d
