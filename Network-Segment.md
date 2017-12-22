---
title: Network Segment
date: 2017-12-02 12:46:07
tags:
    - Deep Learning
    - Python
---


**A example of network**

```python
class Network(object):
	def __init__(self, *arg, **kwargs):
		# .. yada yada, initialize weight and biases

	def feedforward(self, a):
		for b, w in zip(self.biases, self.weights):
			a = sigmoid(np.dot(w, a) + b)
	return a
```
