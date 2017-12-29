---
title: CUDA semantics
date: 2017-12-29 11:07:16
tags:
    - Pytorch
    - Python
    - Deep Learning
---

The selected device can be changed with a `torch.cuda.device` context manager.

Cross-CPU operation are note allowed by default, with the only exception of `copy_()`.

```python
x = torch.cuda.FloatTensor(1)
y = torch.FloatTensor(1).cuda()

with torch.cuda.device(1):
    # allocates a tensor on GPU 1
    a = torch.cuda.FloatTensor(1)

    # transfers a tensor from CPU to GPU 1
    b = torch.FloatTensor(1).cuda()
    # a.get_device() = b.get_device() == 1

    c = a + b
    # c.get_device() == 1

    z = x + y
    # z.get_device() == 0

    # even within a context, ou can give a GPU id to the .cuda call
    d = torch.randn(2).cuda(2)
    # d.get_device() == 2
```

# Memory management

`empty_cache()` can release all unused cached memory from Pytorch so that those can be used by other GPU applications.

# Best practices

## Device-annostic code

A common pattern is to use Python's `argparse` module to read in user arguments, and have a flag that can be used to disable CUDA, int combination with `is_available()`. In the following, `args.cuda` results in a flag that can be used to cast tensors and modules to CUDA if desired:

```python
import argparse
import torch

parser = argparse.ArgumentParser(description = 'PyTorch Example')
parser.add_argument('--disable-cuda', action='store_true', help='Disable CUDA')
args = parser.parse_args()
args.cuda = not args.disable_cuda and torch.cuda.is_available()
```

If modules or tensors need to be sent to the GPU, `args.cuda` can be used as fllows:

```python
x = torch.Tensor(8, 42)
net = Network()
if args.cuda:
    x = x.cuda()
    net.cuda()
```

```python
dtype = torch.cuda.FloatTensor
for i, x in enumerate(train_loader):
    x = Variable(x.type(dtype))
```

`CUDA_VISIBLE_DEVICES`
`torch.cuda.device`

```python
print("Outside device is 0") # On device 0 (default in most scenarios)
with torch.cuda.device(1):
    print("inside device is 1") # on device 1

print("Outside device is still 0") # On device 0
```

```python
x_cpu = torch.FloatTensor(1)
x_gpu = torch.cuda.FloatTensor(1)
x_cpu_long = torch.LongTensor(1)

y_cpu = x_cpu.new(8, 10, 10).fill_(0.3)
y_gpu = x_gpu.new(x_gpu.size()).fill_(-5)
y_cpu_long = x_cpu_long.new([[1, 2, 3]])
```

```python
x_cpu = torch.FloatTensor(1)
x_gpu = torch.cuda.FloatTensor(1)

y_cpu = torch.ones_lick(x_cpu)
y_gpu = torch.zeros_like(x_gpu)
```

# Use pinned memory buffers

- `pin_memory()`
- `cuda(async = True)`
- `pin_memory = True`

# Use `nn.DataParallel` instead of multiprocessing
