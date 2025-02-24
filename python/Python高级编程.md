# Python高级编程

## class

**class Paper**：

```python
class Paper:
    """
    Paper Object
    """
    def __init__(self, title, author):
        self.title = title
        self.author = author
paper = Paper("Attention is all you need", "***")
```

### basic built-in attributes

①`__class__`: For an instance of a class, the `__class__` attribute points to the class that the instance is an object of.

**example**：

```python
paper.__class__
```

**output**：

```plaintext
<class '__main__.Paper'>
```

②`__dict__`: In Python, the `__dict__` attribute is a special attribute that holds the dictionary of an object's (or class's) attributes and methods.

**example**：

```python
p.__dict__
```

**output**：

```python
{'title': "Attention is all you need", "author": "***"}
```

**dynamic modification**：

```python
paper.__dict__["author"] = "**"
```

③`__name__`: The name of the class. **Only valid for classes, not instances.**

**example**：

```python
Paper.__name_
```

**output**

```python
Paper
```

④`__module__`: 定义类的模块名。

**example**：

```python
Paper.__module__  # 当作为主程序运行时，eg. python paper.py 显示main， 否则为模块名
```

**output**：

```python
__main__
```

`__bases__`: 类的基类元组，表示该类继承的所有父类。

`__doc__`: 类的文档字符串，用于记录类的功能描述，文档字符串是定义在模块、类、函数或方法的第一行字符串，用于描述它们的用途，用三引号`"""`包裹。如果没有文档字符串，\__doc__方法返回为None。

PEP 257 是 Python 的文档字符串风格指南，建议使用统一的格式：类的文档字符串应描述该类的用途。方法和函数的文档字符串应说明用途、参数、返回值和异常。

示例：

```python
Paper.__doc__
```



**output**:

```python
Paper Object
```



### Instance method of class

`__init__`:

`__new__`:

`__call__`: 使类能像函数一样被调用

**example**：

```python
class Paper():
    ...  # 省略
    def __call__():
        print("Title:", self.title)
        print("Author:", self.author)
paper()
```

**output**:

```python
"Title: Attention is all you need"
"Author: **"
```



`__str__`: 定义`str()`与`print()`的输出结果。

`__repr__`: 定义`repr()`的输出结果。

`__len__`: 

`__item__`: 

### Attribute operation method

`__getattr__(self, name)`：在访问不存在的属性时自动调用。

`__setattr__(self, name, value)`：为属性赋值时自动调用。

`__delattr__(self, name)`：删除属性时自动调用。

`__getattribute__(self, name)`：访问任意属性时自动调用（包括存在的属性），比 __getattr__ 更早调用。

## Iterator

迭代器是一个可以逐个返回元素的对象，它是实现迭代协议的对象。一般列表、字典、字符等等都是可迭代对象

`__iter__()`: 返回迭代器对象本身，通常在 `for` 循环中会首先调用该方法。

`__next__()`: 返回下一个元素，直到没有元素可以返回时引发 `StopIteration` 异常。

## Generator

## Decorator

A **decorator** in Python is a design pattern that allows you to modify the behavior of a function or a method. It's essentially a function that takes another function as an argument and extends or alters its behavior without explicitly modifying the original function. Firstly, the first focus should be on `wraps` and `Callable`

:pushpin: wraps​

```python
from typing import Callable
from functools import wraps


class MilvusTransaction:
    """
    MilvusTransaction:
        This class is used for the Milvus rollback function to handle exceptions and prevent data corruption.

    Method:

    Attribute:
        rollback_on_failure: if rollback on failure, default True 

    Example:
        @MilvusTransaction(rollback_on_failure=True)

    """
    def __init__(self, rollback_on_failure):
        self.rollback_on_failure = rollback_on_failure
    
    def __call__(self, f: Callable):
        @wraps(f)
        async def wrapper(*args, **kwargs):
            try:
                result = await f(*args, **kwargs)
                pass
            exception RollbackException as e:
                pass
        return wrapper
```



