---
title: xmake试用
date: 2019-03-24 01:29:10
tags:
    - C++
---

## xmake还是挺好玩的

废话，试用了 xmake ，因为之前学 `cmake` 失败，就想找个简单的工具。

`xmake`应算是我目前遇到的最好的`C++`构建工具。

关于我学`make`的心路历程不再，关于`make`的书良莠不齐，只推荐陈硕的那本。其余比如[这本](https://book.douban.com/subject/1992868/)只能望洋兴叹了。用`make`编译过两个小项目，现在想起来都是不好的回忆。`make`断断续续学了好几周，现在全部忘光了。

![p](https://img3.doubanio.com/view/subject/l/public/s2987431.jpg)

## xmake

xmake 绝不完美，这家伙坑居多。

比如添加头文件不能用`add_files`，偏要用`add_headers`，后来又变成了`add_headerfiles`，一言难尽。找这种 [ Bug ](https://github.com/xmake-io/xmake/issues/291) 能吐血。

另外一个缺点就是 [ 文档 ](https://xmake.io/#/home) 比较落后，很多东西 GitHub 上更新了但是文档没跟上。

## 关于使用

我用的最多的几条命令

```shell
xmake   # 构建

# 中间改一改 xmake.lua

# 跑起来
xmake run

# 看看为什么失败了
xmake f -c
```

### 添加静态库

```make
target("library")
    set_kind("static")
    add_files("src/library/*.c")
```

### 添加动态库

照葫芦画瓢

```make
target("library")
    set_kind("shared")
    add_files("src/library/*.c")
```

### 可执行文件

```make
target("test")
    set_kind("binary")
    add_files("src/*c")
    add_deps("library")
```
