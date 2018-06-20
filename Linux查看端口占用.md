---
title: Linux查看端口占用
date: 2018-06-20 16:47:56
tags:
    - Linux
---

## 使用`lsof`

list open files
```sh
lsof -i:your_port
```

## `netstat`

```sh
netstat -anp | grep your_port
```

- `-t`: TCP
- `-u`: UDP
- `-l`: 仅显示LISTEN状态的套接字
- `-a`: all
- `-n`: 不进行DNS解析
- `-p`: 显示进程标识符和程序名
