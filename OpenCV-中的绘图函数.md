---
title: OpenCV 中的绘图函数
date: 2017-12-29 15:06:44
tags:
    - OpenCV
    - Python
---

- `cv2.line()`
- `cv2.circle()`
- `cv2.rectangle()`
- `cv2.ellipse()`
- `cv2.putText()`

---

# 线

```python
import numpy as np
import cv2

img = np.zeros((512, 512, 3))
img = cv2.line(img, (0, 0), (511, 511), (255, 0, 0), 5)
cv2.imshow('img', img)
cv2.waitKey(0)
cv2.destroyAllWindows()
cv2.imwrite('line.jpg', img)
```
{% asset_img line.jpg line %}

# 矩形

```python
img = cv2.rectangle(img, (384, 0), (510, 128), (0, 255, 0), 3)
```

{% asset_img rect.jpg rect %}

# 圆

```python
img = cv2.circle(img, (447, 63), 63, (0, 0, 255), -1)
```

{% asset_img circle.jpg circle %}

# 椭圆

```python
img = cv2.ellipse(img, (256, 256), (100, 50), 0, 0, 180, (255, 0, 0), -1)

ellipse(img, center, axes, angle, startAngle, endAngle, color[, thickness[, lineType[, shift]]]) -> img  or  ellipse(img, box, color[, thickness[, lineType]]) -> img
```

{% asset_img ellipse.jpg ellipse %}

# 多边形

```python
pts = np.array([[10, 5], [20, 30], [70, 20], [50, 10]])
pts = pts.reshape((-1, 1, 2))
img = cv2.polylines(img, [pts], True, (0, 255, 255))

polylines(img, pts, isClosed, color[, thickness[, lineType[, shift]]]) -> img
```

{% asset_img poly.jpg poly %}

# 在图片上添加文字

```python
font = cv2.FONT_HERSHEY_SIMPLEX
cv2.putText(img, 'OpenCV', (10, 500), font, 4 ,(255, 255, 255 ), 2, cv2.LINE_AA)
```

{% asset_img putText.jpg putText %}
