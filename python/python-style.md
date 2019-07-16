---
title: python style
tags:
  - 胡思乱想
categories:
  - python
date: 2019-06-13 15:50:32
---

参考[这里](https://zh-google-styleguide.readthedocs.io/en/latest/google-python-styleguide/python_style_rules/)


# **Python之父Guido推荐的规范**

| Type                       | Public             | Internal                                                          |
|----------------------------|--------------------|-------------------------------------------------------------------|
| Modules                    | lower_with_under   | _lower_with_under                                                 |
| Packages                   | lower_with_under   |                                                                   |
| Classes                    | CapWords           | _CapWords                                                         |
| Exceptions                 | CapWords           |                                                                   |
| Functions                  | lower_with_under() | _lower_with_under()                                               |
| Global/Class Constants     | CAPS_WITH_UNDER    | _CAPS_WITH_UNDER                                                  |
| Global/Class Variables     | lower_with_under   | _lower_with_under                                                 |
| Instance Variables         | lower_with_under   | _lower_with_under (protected) or __lower_with_under (private)     |
| Method Names               | lower_with_under() | _lower_with_under() (protected) or __lower_with_under() (private) |
| Function/Method Parameters | lower_with_under   |                                                                   |
| Local Variables            | lower_with_under   |                                                                   |
