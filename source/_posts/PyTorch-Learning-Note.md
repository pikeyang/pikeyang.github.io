---
title: PyTorch-Learning-Note
date: 2021-10-07 11:22:46
tags:
- Deep Learning
- PyTorch
category: PyTorch
cover: https://yang-img-weng.oss-cn-hangzhou.aliyuncs.com/images/202110071124255.jpeg
---

<!DOCTYPE html>
<html>
<head>
    <style>
        mark {
            background-color:#ffff00 ; font-weight:bold;
        }
    </style>
# 数据操作

## 改变形状

### view

​		`view`不会真正地改变tensor的形状，而是改变了观察tensor的视角。<mark>注意`view()`返回的新`Tensor`与源`Tensor`虽然可能有不同的`size`，但是是共享`data`的，也即更改其中的一个，另外一个也会跟着改变。</mark>

### reshape

​		`reshape`不能保证返回的是源数据的副本还是源数据的不同视角（和view的操作效果相同），所以一般情况下不推荐使用`reshape`。

### clone

​		因为`reshape`的不确定性，所以推荐这种用法`x.clone().view()`

> 使用`clone`还有一个好处是会被记录在计算图中，即梯度回传到副本时也会传到源`Tensor`。

### squeeze

```python
torch.squeeze(input, dim=None, out=None)
将输入张量形状中的1 去除并返回。 如果输入是形如(A×1×B×1×C×1×D)，那么输出形状就为： (A×B×C×D)
当给定dim时，那么挤压操作只在给定维度上。例如，输入形状为: (A×1×B), squeeze(input, 0) 
将会保持张量不变，只有用 squeeze(input, 1)，形状会变成 (A×B)。
 
注意： 返回张量与输入张量共享内存，所以改变其中一个的内容会改变另一个。
 
参数:
 
input (Tensor) – 输入张量
dim (int, optional) – 如果给定，则input只会在给定维度挤压
out (Tensor, optional) – 输出张量
例子：
 
>>> x = torch.zeros(2,1,2,1,2)
>>> x.size()
(2L, 1L, 2L, 1L, 2L)
>>> y = torch.squeeze(x)
>>> y.size()
(2L, 2L, 2L)
>>> y = torch.squeeze(x, 0)
>>> y.size()
(2L, 1L, 2L, 1L, 2L)
>>> y = torch.squeeze(x, 1)
>>> y.size()
(2L, 2L, 1L, 2L)
```

### item

​		`item()`可以将一个标量`Tensor`转换成一个Python number

```python
x = torch.randn(1)
print(x)
print(x.item())
```

```
tensor([2.3466])  #result
2.3466382026672363
```

## 运算的内存开销

​		`y = x + y`这样的运算是会新开内存的，然后将`y`指向新内存。为了演示这一点，我们可以使用Python自带的`id`函数：如果两个实例的ID一致，那么它们所对应的内存地址相同；反之则不同。

```python
x = torch.tensor([1, 2])
y = torch.tensor([3, 4])
id_before = id(y)
y = y + x   # y[:] = y + x
print(id(y) == id_before) # False 
```

​		为了减小内存开销，我们可以使用一些`inplace`的操作，比如`+=`(也即`add_()`)或者`torch.add(x, y, out=y)`还有`y[:] = y + x`

>虽然`view`返回的`Tensor`与源`Tensor`是共享`data`的，但是依然是一个新的`Tensor`（因为`Tensor`除了包含`data`外还有一些其他属性），二者id（内存地址）并不一致。

## `Tensor`和`NumPy`相互转换

​		`numpy()`和`from_numpy()`将`Tensor`和`NumPy`中的数组相互转换。但是需要注意的一点是： **这两个函数所产生的的`Tensor`和`NumPy`中的数组共享相同的内存（所以他们之间的转换很快），改变其中一个时另一个也会改变！！！**

​	此外还可以使用`tensor()`将`NumPy`转为`Tensor`，但是这个过程是数组的拷贝，返回的`Tensor`和原来不共享内存。

## 矩阵乘法

```python
# This computes the matrix multiplication between two tensors. y1, y2, y3 will have the same value
y1 = tensor @ tensor.T
y2 = tensor.matmul(tensor.T)

y3 = torch.rand_like(tensor)
torch.matmul(tensor, tensor.T, out=y3)


# This computes the element-wise product. z1, z2, z3 will have the same value
z1 = tensor * tensor
z2 = tensor.mul(tensor)

z3 = torch.rand_like(tensor)
torch.mul(tensor, tensor, out=z3)
```



# 梯度

## 修改`tensor`

​		如果我们想要修改`tensor`的数值，但是又不希望被`autograd`记录（即不会影响反向传播），那么我么可以对`tensor.data`进行操作。

```python
x = torch.ones(1,requires_grad=True)

print(x.data) # 还是一个tensor
print(x.data.requires_grad) # 但是已经是独立于计算图之外

y = 2 * x
x.data *= 100 # 只改变了值，不会记录在计算图，所以不会影响梯度传播

y.backward()
print(x) # 更改data的值也会影响tensor的值
print(x.grad)
```

```
tensor([1.])
False
tensor([100.], requires_grad=True)
tensor([2.])
```

# 模型

## nn

> 注意：`torch.nn`仅支持输入一个batch的样本不支持单个样本输入，如果只有单个样本，可使用`input.unsqueeze(0)`来添加一维。

### nn和functional

| torch.nn.X                                          | torch.nn.functional.X                           |
| --------------------------------------------------- | ----------------------------------------------- |
| 是 类                                               | 是函数                                          |
| 结构中包含所需要初始化的参数                        | 需要在函数外定义并初始化相应参数,并作为参数传入 |
| 一般情况下放在_init_ 中实例化,并在forward中完成操作 | 一般在_init_ 中初始化相应参数,在forward中传入   |

## init

​		在使用模型之前，我们需要初始化模型参数。

```python
from torch.nn import init

init.normal_(net[0].weight, mean=0, std=0.01)
init.constant_(net[0].bias, val=0)  # 也可以直接修改bias的data: net[0].bias.data.fill_(0)
```



