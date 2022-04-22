---
title: distance measures
date: 2022-04-22 20:05:44
tags: Deep Learning
categories: Deep learning
cover: https://gitee.com/cd-yang/pic/raw/master/img/202204222010766.png
---

# 距离公式分析

![](https://gitee.com/cd-yang/pic/raw/master/img/202204222010766.png)

# 欧式距离

$$
D(x,y)=\sqrt{\sum^{n}_{i=1}{(x_i-y_i)^2}}
$$

## 缺陷

- 欧式距离在衡量特征之间的距离时，需要考虑特征的分布范围。一般来讲使用欧式距离时需要标准化（$z\sim N(0, 1)$）。
- 欧式距离对于高维特征来说并非好的选择。（高纬度和低纬度分布的差别）

>如果我们通过将超球面刻在超立方体中来近似超球面，那么在高维中，几乎所有超立方体的体积都在超球面之外。 这对于机器学习来说是个坏消息，因为我们通常使用其中一种类型的形状近似另一种类型的形状。

## 使用场景

- 低维数据，向量的大小很重要
  - kNN
- 直观，易于实现使其使用广泛

# Cosine 相似度

$$
D(x,y)=cos(\theta)=\frac{x\cdot y}{||x||\cdot ||y||}
$$

## 缺陷

- 没有将特征的大小考虑在内
  - 比如$(1,1) and (2,2)$

## 使用场景

- 高维数据
- 特征的大小不重要时
  - 一个词在一篇文章中出现的次数多于在另一篇文章中的出现次数，并不能说明它和第一篇文章的关联度大，还有可能是文章长度问题

# 汉明距离



# Reference

[9 Distance Measures in Data Science](https://towardsdatascience.com/9-distance-measures-in-data-science-918109d069fa)
