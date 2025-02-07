# Pytorch

:white_check_mark: torch.arange()​

`torch.range()` is the function that was part of Pytorch's API but has been deprecated since torch1.2.0. It is recommended to use `torch.arange()` instead of generating sequences of numbers. it is similar to `numpy.arange` and generate evently spaced values over a specified range. The following is the detailed instruction of `torch.range()`

:pencil: **Syntax**:

```python
torch.arange(start=0, end, step=1, dtype=None, device=None, layout=torch.strided, pin_memory=False)
```

