---
title: CV-some-tips
date: 2021-10-29 21:21:00
tags:
- Deep Learning
- Computer Vision
- CNN
categories:
- Deep Learning
cover: https://raw.githubusercontent.com/pikeyang/iamage/master/img/202210141605871.png?token=AOH5QIC5TDKHGOU4AWF5DQDDJEMHW
---



# [【CNN】理解卷积神经网络中的通道 channel_scxyz的博客-CSDN博客_cnn通道](https://blog.csdn.net/sscc_learning/article/details/79814146)

![cnn](https://raw.githubusercontent.com/pikeyang/iamage/master/img/202210141605871.png?token=AOH5QIC5TDKHGOU4AWF5DQDDJEMHW)

![多个卷积核](https://gitee.com/cd-yang/pic/raw/master/img/202110300953770.png)

# 1x1conv

Suppose that I have a conv layer which outputs an (N,F,H,W)(N,F,H,W) shaped tensor where:

- NN is the batch size
- FF is the number of convolutional filters
- H,WH,W are the spatial dimensions

Suppose this output is fed into a conv layer with F1F1 1x1 filters, zero padding and stride 1. Then the output of this 1x1 conv layer will have shape (N,F1,H,W)(N,F1,H,W).

So 1x1 conv filters can be used to change the dimensionality in the filter space. If F1>FF1>F then we are increasing dimensionality, if F1<FF1<F we are decreasing dimensionality, in the filter dimension.

