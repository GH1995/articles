---
title: 'OpenCV: 4 图片'
date: 2017-12-26 13:03:14
tags:
    - OpenCV
    - Python
---

- `cv2.imread()`
- `cv2.imshow()`
- `cv2.imwrite()`

---

# 读入图片 `cv2.imread()`

- `cv2.IMREAD_COLOR`
- `cv2.IMREAD_GRAYSCALE`

---

# 显示图片

- `cv2.waitKey()`
- `cv2.destroyAllWindows()`

---

# 保存图片

`cv2.imwrite('example.png', img)`

```python
#!/usr/bin/env python3
# coding: utf-8

import numpy as np
import cv2

img = cv2.imread('example.jpg', 0)
cv2.imshow('image',img)
cv2.waitKey(0)
# 64 bit os
# cv2.waitKey(0)&0xFF
cv2.destroyAllWindows()
```
