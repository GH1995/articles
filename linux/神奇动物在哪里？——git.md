---
title: 神奇动物在哪里？：护树罗锅——git
tags:
  - git
categories:
  - linux
date: 2019-06-27 16:46:48
---

{% cq %}
护树罗锅 (Bowtruckle)是一种很难被发现、手掌大小的以昆虫为食的动物。护树罗锅的两只手上都有长而锋利的手指，眼睛为褐色，从外表看像是由树皮和小树枝构成的，很容易伪装起来。
{% endcq %}

## 基本配置

你是不是也在头疼每次提交的 commit 记录怎么写？这里推荐在`.gitconfig`里添加一条自定义命令。

```
yolo = !git commit -m \"$(curl -s whatthecommit.com/index.txt)\"
```

```
git config --global credential.helper store # 记录手动输入的账号和密码
```

## 提交修改

有了`git yolo`还要什么自行车

## 单个文件的历史

```
# 显示某个文件的历史版本的所有改动
git log --follow -p [file]
git whatchanged -p [file]

# 显示指定文件的每一行内容的作者和修改时间
git blame [file]
```

## 统计每个作者的提交情况
```
git shortlog -sn
```

## diff

最重要的三个，其余的用到了再背
```
# 查看工作区域暂存区的差异
git diff

# 查看暂存区与版本库的差异
git diff --cached

# 查看 暂存区 与 HEAD 的差异
git diff HEAD  
```

## 找回丢失的提交

`git reflog` 可以列出 HEAD 曾经指向过的一系列 commit。 该命令可用于在错误操作时找回丢失的提交。 

如果引起 commit 丢失的原因并没有记录在 `reflog` 中，则使用 `git fsck` 工具，该工具会检查仓库的数据完整性。

```
git reflog

git fsck --full

git fsck --lost-found
```

## cherry-pick

`git cherry-pick` 能够将其他分支上的某个或者多个提交应用到当前分支中，而不必将整个分支的内容都合并到当前分支。

## revert

`git revert` 用于只撤销历史上的某个或者某几个提交，而不改变前后的提交历史，并且将撤销操作作为一次最新的提交。

## 整理提交历史

1. 修改最新一个提交
2. 整理某一提交之后的所有提交
3. 修改历史上任意提交

## 撤销暂存区

如果执行 `git add` 操作后发现修改的内容有误，需要重新修改时，可以使用 `reset` 撤销提交到暂存区的修改到工作区中。

## 代码回滚

`checkout` `reset` `revert`

## 拯救出错的 rebase 操作

1. 如果分支上有新的提交还没 push，可以用 reflog 查看操作历史，然后用 reset 回退
2. 如果分支上没有提交，或者不关心这个分支上新的提交，只是想做 rebase 操作，那么可以先切换到别的分支，把 rebase 出问题的分支删掉再重新 rebase
3. 或者更简单一些，让 HEAD 指向远程分支的 HEAD


## 分支管理

`git branch`

还有一个分支替换功能

## 何时保留分支历史

```
# 将提交约线图平坦化
git pull --rebase

# 刻意制造分叉
git merge --no-ff
```

## Tags

Git 有两种类型的标签，即 轻量级的(lightweight) 和 含附注的(annotated)。轻量级类似一个指针，其指向一个特定的提交对象。含附注标签，是存储在仓库中一个独立的对象，其有自身的校验和信息，且包含标签的名字，邮件地址和日期，以及标签说明，创建含附注类型的标签需要指定 -a 参数（取 annotated 的首字母）。

## 远程仓库
```
# 修改远程仓库地址
git remote set-url [remote] [url]
```

## 清理工作区

使用 `git clean` 可以清除工作区未被添加到索引中的文件。

[More](http://blog.konghy.cn/2018/05/06/git-notes/)

