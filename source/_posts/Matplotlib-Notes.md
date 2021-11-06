---
title: Matplotlib Notes
date: 2021-11-06 20:59:38
tags: 
- Matplotlib
- Python
categories: Python
cover: https://gitee.com/cd-yang/pic/raw/master/img/202111062128916.png
---



# Figure Axis

![v2-896219515a8d7f28fdcb757e0ae81e8d_1440w](https://gitee.com/cd-yang/pic/raw/master/img/202111062131751.jpg)![preview](https://gitee.com/cd-yang/pic/raw/master/img/202111062127754.jpeg)



# plt and ax

​		在日常的使用中plt和ax经常混淆，需要记录一下。

## plt

```python
plt.figure()
plt.plot([1,2,3],[4,5,6])
plt.show()
```

![image-20211106213655889](https://gitee.com/cd-yang/pic/raw/master/img/202111062136946.png)



## ax

```python
fig,ax = plt.subplots()
ax.plot([1,2,3],[4,5,6])
plt.show()
```

![image-20211106213728477](https://gitee.com/cd-yang/pic/raw/master/img/202111062137539.png)

## 区别

​		两种情况下，得到的图像是一致的但是原理却不同。

​		从第一种方式的代码来看，先生成了一个`Figure`画布，然后在这个**画布上隐式生成一个画图区域进行画图**。

​		第二种方式同时生成了`Figure`和`axes`两个对象，然后用`ax`对象在**其区域内**进行绘图。如果从**面向对象编程**（对理解`Matplotlib`绘图很重要）的角度来看，显然第二种方式更加易于解释，生成的`fig`和`ax`分别对画布`Figure`和绘图区域`Axes`进行控制，第一种方式反而显得不是很直观，如果涉及到子图**零部件的设置**，用第一种绘图方式会很难受。

​		在实际绘图时，也更**推荐使用第二种方式。**

## subplot的绘制

![image-20211106213950817](https://gitee.com/cd-yang/pic/raw/master/img/202111062139856.png)

# Reference

1. [Matplotlib中的plt和ax都是啥？ - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/137057629)



