---
title: Numpy-Note
date: 2021-10-09 12:15:13
tags:
- Python
- Random
- Numpy
category: Numpy
cover: https://gitee.com/cd-yang/pic/raw/master/img/202110091216761.png 
---

# `Numpy`

​		[官方文档](https://numpy.org/doc/stable/reference/random/generator.html)

# Random

![image-20211009121523706](https://gitee.com/cd-yang/pic/raw/master/img/202110091215782.png)



## `numpy.random.normal`

> Parameters
>
> - **`loc`**: float or array_like of floats
>
>   Mean (“`centre`”) of the distribution.
>
> - **scale**: float or array_like of floats
>
>   Standard deviation (spread or “width”) of the distribution. Must be non-negative.
>
> - **size**: `int` or tuple of`ints`, optional
>
>   Output shape. If the given shape is, `e.g`., `(m, n, k)`, then `m * n * k` samples are drawn. If size is `None` (default), a single value is returned if `loc` and `scale` are both scalars. Otherwise, `np.broadcast(loc, scale).size` samples are drawn.
>
> Returns
>
> - **out**: `ndarray` or scalar
>
>   Drawn samples from the parameterized normal distribution.

![image-20211009124146088](https://gitee.com/cd-yang/pic/raw/master/img/202110091241117.png)

​		相比于`random.randn`只能产生标准的正态分布，normal中，可以自己选择，`loc`和`scale`
$$
loc\to\mu \\
scale\to\sigma
$$
