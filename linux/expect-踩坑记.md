---
title: expect 踩坑记
tags:
  - Linux
categories:
  - linux
date: 2019-07-10 18:12:05
---

expect 是一个攻来处理交互的命令。简单来说，我们可以用 ssh 连上另一台 Linux，连上去这个动作好做。 但是连上了之后，我们如何在这台机器上执行命令呢？

> Expect  is  a program that "talks" to other interactive programs according to a script.
> expect 是 tcl 的一个套装，tcl 因为**模式-动作**而强大。

- `send` 向进程发送字符串
- `expect` 从进程接收字符串
- `spawn` 启动新的进程
- `interact` 允许用户交互

## `send` 没什么好说的，看例子即可

send "hello world\n"

## `expect` 接收正则表达式参数，这里用到了 tcl 相关的知识了

```
expect "hi\n"
send "you typed <$expect_out(buffer)>"
send "but I only expected <$expect_out(0,string)>"
```

- `<$expect_out(buffer)>` 存储了所有输入
- `<$expect_out(0,string)>` 存储了输入中的匹配行

接下来是模式动作部分
**重点**

```
expect {
    "hi" { send "You said hi\n" }
    "hello" { send "Hello yourself\n" }
    "bye" { send "That was unexpected\n" }
}
```

## spawn，启动新的进程，把这个进程看成黑盒，然后用 send 和 expect 同这个进程交互

```
#!/usr/bin/expect

# filename: test.sh

spawn ssh root@192.168.22.194
expect "*password:"
send "123\r"
expect "*#"
interact
```

**注意这里有个坑！**

不要用 bash 去运行这个脚本，要用 `expect` ！！


## interact，干完活之后依然停在 spawn 启动的 shell 上

以便执行后续命令


## 如何从跳板机登录到开发机呢？

```
set timeout -1
set ip $1
set password $2

spawn ssh -p xxx username@ip

expect {
    "*yes/no"       { send "yes\r"; exp_continue  }
    "*password:"    { send "$password\r"  }
}
interact
```

参考文章：https://www.cnblogs.com/lzrabbit/p/4298794.html
