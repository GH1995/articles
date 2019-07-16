---
title: Keras源码漫谈(1)
tags:
  - keras
  - deep learning
categories:
  - tensorflow
date: 2019-06-27 15:02:37
---

## `setup.py`

这篇文章主要参考这里[^1]，写的非常好，推荐看一下原文。搞懂 Python 的 package 是如何构建的。

`distutils` 和 `setuptools` 差不多，功能较少，现在用的比较少了。下文主要介绍 setuptools 。

## 包格式

wheel 和 Egg 本质上都是 zip 包，推荐使用 wheel。

## `setup.py` 文件内容

首先需要`setup`函数

```
from setuptools import setup
from setuptools import find_packages
```
这里导入了两个东西，`find_packages`稍后再讲。

`setup`函数里面的内容很好懂。

```
setup(name='Keras', # 包名
      version='2.2.4', # 版本
      ...
      install_requires=['numpy>=1.9.1',
      ...
      extras_require={
      ...
      classifiers=[
      ...
      packages=find_packages())
```


## 参数概述

常见的没什么好说的，见[这里](https://setuptools.readthedocs.io/en/latest/setuptools.html#metadata)

里面有几个要注意

### `find_packages()`【重点】

默认在与 `setup.py` 文件同一目录下搜索各个含有 `__init__.py` 的目录做为要添加的包。 

### 数据文件
略

### 生成脚本

略

### `ext_modules`

用于构建 C 和 C++ 扩展扩展包。略。

### `zip_safe`

无脑`False`即可

### 自定义命令

提供`-h`选项

```
from setuptools import setup, Command
```

### 依赖关系【重点】

指定该参数后，在安装包时会自定从 pypi 仓库中下载指定的依赖包安装。

看一下 keras 依赖的包
```
install_requires=['numpy>=1.9.1',  # 计算
                'scipy>=0.14', # 计算
                'six>=1.9.0', # 处理兼容性
                'pyyaml', # 处理格式
                'h5py', # 存储
                'keras_applications>=1.0.6', # 自家的
                'keras_preprocessing>=1.0.5'], # 自家的
```

这里还有一个参数`extras_require`，表明当前包的高级/额外特性需要依赖的分发包

### 分类关系

`classifiers`
讲讲包的成熟度，目标用户，类型，许可证，目标 Python 版本

## setup.py 命令

- bulid
- sdist
- bdist
- install
- develop
- register/upload

## `setup.cfg` 文件

提供 `setup.py` 的默认参数

keras 没什么默认参数


[^1]: http://blog.konghy.cn/2018/04/29/setup-dot-py/
