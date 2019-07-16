---
title: Autograd mechanics
tags:
  - Pytorch
  - Python
  - Deep Learning
categories:
  - pytorch
date: 2017-12-28 15:31:03
---

# Excluding subgraphs from backward

Every Variable has two flags: `requires_grad` and `volatile`.

## `requires_grad`

If there's a single input to an operation that requires gradient, its output will also require gradient.

```python
x = Variable(torch.randn(5, 5))
y = Variable(torch.randn(5, 5))
y = Variable(torch.randn(5, 5), requires_grad = True)
a = x + y
a.requires_grad    # False
b = a + z
b.requires_grad     # True
```

```python
model = torchvision.models.resnet18(pretrain=True)
for param in model.parameters():
    param.requires_grad = False

model.fc = nn.Linear(512, 100)
optim.SGD(model.fc.parameters(), lr=1e-2, momentum=0.9)
```

## `volatile`

Volatile is recommended for purely inference mode, when you're sure you won't be even calling `.backward()`. `volatile` also determines that `requires_grad` is `False`.

If there's even a single volatile input to an operation, its output is also going to be volatile.

```python
regular_input = Variable(torch.randn(1, 3, 277, 277))
volatile_input = Variable(torch.randn(1, 3, 277, 277))
model = torchvision.models.resnet18(pretrain=True)

model(regular_input).requires_grad  # True
model(volatile).requires_grad       # False

model(volatile_input).volatile      # True
model(volatile_input)grad_fn is None # True
```

# How autograd encodes the history

# In-place operations on Variables

# In-place correctness checks
