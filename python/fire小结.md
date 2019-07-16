---
title: fire小结
tags:
  - Python
categories:
  - python
date: 2019-06-17 10:46:36
---

## 单个函数

```python
def cal_days():
    pass

if __name__ == '__main__':
    fire.Fire(cal_days) # 注意这里
```

```shell
python test.py 
```

## 多个函数


```python
def cal_days_1():
    pass

def cal_days_2(days):
    pass

if __name__ == '__main__':
    fire.Fire() # 注意这里是空的
```

```shell
python test.py cal_days_2 20190617
```

## 对象

```python
class DateCompare(object):
    def cal_days(self, date):
        pass

if __name__ == "__main__":
    fire.Fire(DateCompare)
```

```shell
python test.py cal_days 20190617
```

以上就是python `fire`的用法。
