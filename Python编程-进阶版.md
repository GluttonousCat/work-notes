# Python编程-进阶版

## 类的内置属性与方法

**示例类Paper**：

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

### 基本内置属性

①`__class__`: 返回当前实例所属的类。

**示例**：

```py
paper.__class__
```

**输出**：

```python
<class '__main__.Paper'>
```

②`__dict__`: 对象的属性字典，包含了类实例的所有属性及其值。可用于动态访问和修改属性。

**示例**：

```python
p.__dict__
```

**输出**：

```json
{'title': "Attention is all you need", "author": "***"}
```

**动态修改**：

```python
paper.__dict__["author"] = "**"
```

③`__name__`: 类的名称。**仅对类生效，不对实例生效**

**示例**：

```python
Paper.__name__
```

输出

```python
Paper
```

④`__module__`: 定义类的模块名。

**示例**：

```python
Paper.__module__  # 当作为主程序运行时，eg. python paper.py 显示main， 否则为模块名
```

**输出**：

```python
__main__
```

`__bases__`: 类的基类元组，表示该类继承的所有父类。

`__doc__`: 类的文档字符串，用于记录类的功能描述。



### 常见内置方法

`__init__`:

`__new__`:

`__call__`: 使类能像函数一样被调用

**示例**：

```python
class Paper():
    ...  # 省略
    def __call__():
        print("Title:", self.title)
        print("Author:", self.author)
paper()
```

**输出**:

```python
"Title: Attention is all you need"
"Author: **"
```



`__str__`: 定义`str()`与`print()`的输出结果。

`__repr__`: 定义`repr()`的输出结果。

`__len__`: 

`__item__`: 

### 属性访问控制

`__getattr__(self, name)`：在访问不存在的属性时自动调用。

`__setattr__(self, name, value)`：为属性赋值时自动调用。

`__delattr__(self, name)`：删除属性时自动调用。

`__getattribute__(self, name)`：访问任意属性时自动调用（包括存在的属性），比 __getattr__ 更早调用。



## 迭代器

迭代器是一个可以逐个返回元素的对象，它是实现迭代协议的对象。一般列表、字典、字符等等都是可迭代对象

`__iter__()`: 返回迭代器对象本身，通常在 `for` 循环中会首先调用该方法。

`__next__()`: 返回下一个元素，直到没有元素可以返回时引发 `StopIteration` 异常。





## 生成器

## 装饰器



## 

