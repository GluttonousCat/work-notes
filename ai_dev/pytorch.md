# Pytorch:zzz:

:white_check_mark: torch.arange()​

`torch.range()` is the function that was part of Pytorch's API but has been deprecated since torch1.2.0. It is recommended to use `torch.arange()` instead of generating sequences of numbers. it is similar to `numpy.arange` and generate evently spaced values over a specified range. The following is the detailed instruction of `torch.range()`

:pencil: **Syntax**:

```python
torch.arange(start=0, end, step=1, dtype=None, device=None, layout=torch.strided, pin_memory=False)
```

where:

1. start is the first
1. 2
1. 2

:white_check_mark: torch.size()

In torch, `torch.size()` is used to get the shape(dimension) of a tensor. It is return the `torch.Size` object, which is essentially a tuple of integers representing the size of the tensor along the dimension. 

:pencil: **Syntax**:

```python
import torch

tensor = torch.randn(2, 3)  # generate a tensor which shape is (2,3)
tensor_shape = tensor.size()
'''
tensor([[-0.9704,  0.4572, -0.8191],
        [-0.7813, -1.3181, -0.3200])
torch.Size(2, 3)
'''
# change the shape
tensor = torch.view(3, 2)
tensor_shape = tensor.size()
'''
tensor([[-0.9704,  0.4572], 
        [-0.8191, -0.7813],
        [-1.3181, -0.3200])
'''
```

`torch.size()` can get the int parameter which represents the dimension such as `torch.size(0)` represent the first dimension.  It is similar to the list slice. you can also use the `-1` indicate the last dimension.

