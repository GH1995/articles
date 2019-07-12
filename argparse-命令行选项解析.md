---
title: 'argparse:命令行选项解析'
date: 2019-07-11 15:54:27
tags:
---

![示例文件](https://s2.ax1x.com/2019/07/11/Z27pAP.png)


## 准备：`sys.argv` 和命令行参数

```python
# 输出命令行参数

import sys

def main(argv):
    for i in range(len(sys.argv)):
        print('sys.argv[{0}] = {1}'.format(i, sys.argv[i]))

if __name__ == "__main__":
    main(sys.argv)
```

## 命令行参数解析

Python 使用 `argparse` 模块实现命令行选项和参数的解析。

1. 创建 `ArgumentParser` 对象
2. 添加参数`add_argument()`
3. 解析参数`parse_args()`

```python
import argparse


def main():
    # 创建参数解析器 ArgumentParser 对象
    parser = argparse.ArgumentParser()

    """
    添加参数
    - name 或 flag，指定该参数为名称参数还是选项参数
        - `foo`
        - `-f` 或 `--foo`
    - dest 
    - metavar 在使用方法 usage 中，该参数的名称
    """
    parser.add_argument('-o', dest='outfile', help='output file')
    parser.add_argument(dest='filenames', metavar='filename', nargs='*')

    # 解析参数
    args = parser.parse_args()

    print(args.filenames)
    print(args.outfile)


if __name__ == "__main__":
    main()
```

最终效果如上所示。`argparse`模块太复杂了，我并不建议使用。与之相比，Google 的`fire` 更加简单灵活，我写了一个[简单的教程](https://guanhua.ml/2019/06/17/fire%E5%B0%8F%E7%BB%93/)。
