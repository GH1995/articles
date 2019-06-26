---
title: 'Python decorators '
date: 2019-06-14 12:45:05
tags:
    - Python

---

Python 装饰器强大并且高效，这里我们只需要懂得最基础的用法。
等需要更高阶的用法时，自然会主动学习。


```python
# 装饰器(decorators)
# 这个例子中，beg装饰say
# beg会先调用say。如果返回的say_please为真，beg会改变返回的字符串。
from functools import wraps


def beg(target_function):
    @wraps(target_function)
    def wrapper(*args, **kwargs):
        msg, say_please = target_function(*args, **kwargs)
        if say_please:
            return "{} {}".format(msg, "Please! I am poor :(")
        return msg

    return wrapper


@beg
def say(say_please=False):
    msg = "Can you buy me a beer?"
    return msg, say_please


print(say())  # Can you buy me a beer?
print(say(say_please=True))  # Can you buy me a beer? Please! I am poor :(
```

参见[这里](https://learnxinyminutes.com/docs/zh-cn/python3-cn/)
