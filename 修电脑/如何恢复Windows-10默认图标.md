---
title: 如何恢复Windows 10默认图标
tags:
  - Windows
categories:
  - 修电脑
date: 2018-07-02 01:15:36
---

在使用Windows的时候经常会遇到这种问题：

> 设置了一个程序为错误的默认文件打开方式，现在想将默认打开方式删除掉有没有什么方法

对于这个问题微软官方的[解释](https://answers.microsoft.com/zh-hans/windows/forum/windows_10-other_settings/win10%E6%80%8E%E4%B9%88%E5%8F%96%E6%B6%88%E9%BB%98/34f31f50-1c5d-4996-a876-a07a8f250c0e)是：

> 没有这项功能


事实上我们是可通过修改注册表完成的，以删除`cpp`的默认打开程序为例

1. `win+R`调出运行窗口输入`regedit`
2. 删除`计算机\HKEY_CLASSES_ROOT\.cpp`
3. 删除`计算机\HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\FileExts\.cpp`
4. 重启资源管理器
