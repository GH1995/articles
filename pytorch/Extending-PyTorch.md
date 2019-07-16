---
title: Extending PyTorch
tags:
  - Pytorch
  - Python
  - Deep Learning
categories:
  - pytorch
date: 2017-12-29 12:39:13
---

# Extending `torch.autograd`

Adding operation to `autograd` requires implementing a new `Function` subclass ofr each operation.
Every new function requires you to implement 2 methods:

- `forward()`
- `backward()` - gradient formula. It will be given as many `Variable` arguments as there were outputs, with each of them representing gradient w.r.t. that output.

```python
# Inherit from Function
class LinearFunction(Function):
    # Note that both forward and backward are @staticmethods
    @staticmenthod
    # bias is an optional argument
    def forward(ctx, input ,weight, bias=None):
        ctx.save_for_backward(input, weight, bias)
        output = input.mm(weight.t())
        if bias is not None:
            output += bias.unsqueeze(0).expand_as(output)
        return output

    # This function has only a single output, so it gets only one gradient
    @staticmenthod
    def backward(ctx, grad_output):
        # This is a pattern that is very convenient - at the top of backward
        # unpack saved_tensors and initialize all gradients w.r.t. inputs to
        # None. Thanks to the fact that additional trailing Nones are
        # ignored, the return 
```

```python

```

## Adding a Module

# Writing custom C exensions
