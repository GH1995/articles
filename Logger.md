---
title: Logger
date: 2019-07-29 11:50:25
tags:
---

```
import logging

logging.debug('This is a debug message')
logging.info('This is an info message')
logging.warning('This is a warning message')
logging.error('This is an error message')
logging.critical('This is a critical message')
```

```
logging.basicConfig(level=logging.DEBUG)
logging.debug('This will get logged')
```

低于 debug 下的所有信息都不会被打印出来。


```
logging.basicConfig(filename='app.log', filemode='w', format='%(name)s - %(levelname)s - %(message)s')
logging.warning('This will get logged to a file')
```

```
root - ERROR - This will get logged to a file
```

## 格式化输出

```
logging.basicConfig(format='%(process)d-%(levelname)s-%(message)s')
logging.warning('This is a Warning')
```

```
18472-WARNING-This is a Warning
```

```
logging.basicConfig(format='%(asctime)s - %(message)s', level=logging.INFO)

```
