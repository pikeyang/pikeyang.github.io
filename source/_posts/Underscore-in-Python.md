---
title: Underscore in Python (Python中的下划线)
date: 2022-02-28 16:30:44
tags: Python
categories: Python
---

[TOC]

## 1.在解释器中的使用

​		Python会自动将解释器中最后一个值存储到‘_’中

![截屏2022-02-28 下午4.33.27](https://yang-img-weng.oss-cn-hangzhou.aliyuncs.com/images/202304042209516.png)

## 2.忽略具体值

​		很多情况下，在Python的解包操作中，我们并非需要其中的所有值。这时可以使用‘_’来忽略部分值。

![截屏2022-02-28 下午4.37.12](https://yang-img-weng.oss-cn-hangzhou.aliyuncs.com/images/202304042209988.png)

## 3.在循环中使用

​		这种用法和用法2较为类似。同样为不给某些值具体变量名。

![截屏2022-02-28 下午4.38.40](https://yang-img-weng.oss-cn-hangzhou.aliyuncs.com/images/202304042209216.png)

## 4.分隔数字

​		在使用较长的数字时，可以使用下划线进行分隔。

![](https://yang-img-weng.oss-cn-hangzhou.aliyuncs.com/images/202304042209679.png)

## 5.命名

​		前缀双下划线有实际意义，而其他的都是一种编程约定。

- Single Pre Underscore:- **_variable**

- Signle Post Underscore:- **variable_**

- Double Pre Underscores:- **__variable**

- Double Pre and Post Underscores:- **__variable__**

### 5.1 前缀单下划线

​		单下划线意味着内部使用，类似于Java中的private，在import 操作时不会引入有前缀单下划线的变量或者方法。

### 5.2 后缀单下划线

​		Python中经常遇到命名冲突的场景，后缀单下划线就可以用来解决命名冲突问题。

![截屏2022-02-28 下午4.45.37](https://yang-img-weng.oss-cn-hangzhou.aliyuncs.com/images/202304042209431.png)

### 5.3 前缀双下划线

​		Name magling is the process to overwrite such identifiers in a class to avoid conficts of names between the current class and its subclassed. 名称改写是改写类中变量名的一种行为，用来避免当前类和它的子类命名冲突的问题（避免被重写）。

```python
class Base:
		def __init__(self):
        self.__var = 1

>>> base = Base()
>>>dir(base)
['_Base__var', '__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__']
>>>base.__var
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'Base' object has no attribute '__var'
>>> base._Base__var
1
```

### 5.4 前后双下划线

​		魔术方法。

![截屏2022-02-28 下午4.55.50](https://yang-img-weng.oss-cn-hangzhou.aliyuncs.com/images/202304042209365.png)

## 参考文献

1.[Role of Underscore(_) in Python Tutorial](https://www.datacamp.com/community/tutorials/role-underscore-python#SDON)

