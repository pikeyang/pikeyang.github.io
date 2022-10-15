---
title: Linear Regression
date: 2022-10-14 16:09:52
tags: ML
category: Machine Learning
---

# 线性回归与最小二乘法

## 推导

​		线性回归的公式容易表示为.


$$
Y = X \cdot w    (w_{(p+1) \times 1})
\label{linear}
$$
​		其中，X为$\eqref{eq:X}$ 
$$
X = (x_1, x_2, ..., X_N)^T =
\left( \begin{array}{c}
	x_1^T\\
	x_2^T\\
	\vdots\\
	x_N^T\\
\end{array} \right) = 
\left( \begin{matrix}
	x_{10}&     x_{11}&		x_{12}&		\cdots&		x_{1p}\\
	x_{20}&     x_{21}&		x_{22}&		\cdots&		x_{2p}\\
	\vdots&     \vdots&		\vdots&		&		\vdots\\
	X_{N0}&     x_{N1}&		x_{N2}&		\cdots&		x_{Np}\\
\end{matrix} \right)_{N \times (p+1)}

\label{eq:X}
$$

​		其中，$x_{*0}$为常数1，W的形状为$(p+1, 1)$，$X$为N个样本，p个特征，多出来的常数项用于计算偏置。

$$
Y = \left( \begin{array}{c}
	y_1\\
	y_2\\
	\vdots\\
	y_N\\
\end{array} \right)_{N \times 1}
$$

​		$X$为已知量，那么可以$Y=X\cdot w$ 可以看做关于W的函数，记作：
$$
f(w)=w^Tx
$$


## 最小二乘估计

$$
\mathscr{L}(w) = \sum_{i=1}^{N}||w^T \cdot x_i - y_i|| ^ 2 = \sum_{i=1}^N(w^T \cdot x_i - y_i)^2
$$

$$
\begin{aligned}
\mathscr{L}(w) &= \left( \begin{matrix}  w^Tx_1-y_1&		w^Tx_2-y_2&	\cdots&	w^Tx_n-y_n \end{matrix} \right) \cdot \left( \begin{matrix} w^Tx_1-y_1 \\ w^Tx_2-y_2\\ \vdots \\ w^Tx_n-y_n  \end{matrix} \right)  \\ &= 
(\left( \begin{matrix}  w^Tx_1&		w^Tx_2&	\cdots&	w^Tx_n \end{matrix} \right) -\left( \begin{matrix}y_1& y_2& \cdots& y_N  \end{matrix}\right))
\cdot \left( \begin{matrix} w^Tx_1-y_1 \\ w^Tx_2-y_2\\ \vdots \\ w^Tx_n-y_n  \end{matrix} \right)  \\ &=
(w^T\left( \begin{matrix}  x_1&		x_2&	\cdots&	x_n \end{matrix} \right) -\left( \begin{matrix}y_1& y_2& \cdots& y_N  \end{matrix}\right))
\cdot \left( \begin{matrix} w^Tx_1-y_1 \\ w^Tx_2-y_2\\ \vdots \\ w^Tx_n-y_n  \end{matrix} \right) \\ &=
(w^T X^T - Y^T)(Xw-Y) 
\end{aligned}

\label{lse-matrix}
$$

​	$\mathscr{L}(W)$是一个常数。 
$$
\mathscr{L}(W)=(w^T X^T - Y^T)(Xw-Y) = w^T X^TXw - w^T X^TY - Y^TXw - Y^TY

\label{eq:loss-res}
$$
​		$\mathscr{L}(W)$是一个常数，则$\eqref{eq:loss-res}$ 中每一项都是常数。则：
$$
w^T X^TY = (w^T X^TY)^T = Y^TXw
$$

$$
\mathscr{L}(w)=(w^T X^T - Y^T)(Xw-Y) = w^T X^TXw - 2w^T X^TY - Y^TY
$$
​	
$$
\hat{w} = argmin \mathscr{L}(w)
$$

$$
\frac{\partial \mathscr{L}(w)}{\partial w} = 2X^TXw - 2X^TY = 0
$$

$$
W=\begin{array}{c}
	\underbrace{(X^TX)^{-1}X^T}Y\\
	Pseudo-inverse\\
\end{array}
$$

## 最小二乘法几何解释

<img src="https://raw.githubusercontent.com/pikeyang/iamage/master/img/202210141938336.png?token=AOH5QIEUKNJ4S6RBGFSCJGDDJFFIC" alt="lse-geometry" style="zoom: 13%;" />

​		X中的所有向量可以构成一个向量空间，我们想用Y来表达该空间，但是Y能落到X构成的向量空间中的概率很小，所以只能退而求其次，找到Y在向量空间中的投影$\hat{Y}=Xw$，并使Y和该投影的误差尽量小。

​		由于$\hat{Y}$是Y在向量空间中的投影，所以有$\eqref{eq:projection}$。
$$
X^T \cdot (Y-\hat{Y}) = 0 \\
X^T\cdot(Y-Xw) = 0 \\

\label{eq:projection}
$$

## 最小二乘法的概率解释



​		由于实际数据的随机性，不妨假设$f(w) = w^Tx+\varepsilon, \varepsilon \sim N(0, \sigma^2)$。根据极大似然$p( y_i|x_i;w )$估计有：		
$$
\hat{w} = \underset{w}{argmax}\prod_{i=1}{p( y_i|x_i;w )}
$$
​		在这里，$P(y_i|x_i;w)≤1$，相乘的结果由于取数值小于1的乘积会随着这些项数量的增加而逼近0,容易溢出。所以变换如下：
$$
\begin{aligned}
\hat{w} &= \underset{w}{argmax}\sum_{i=1}\log{p( y_i|x_i;w )} \\ &=
\underset{w}{argmax}\sum_{i=1}{-\log {\sqrt {2\pi}\sigma} - \frac{||w^T \cdot x_i - y_i||^2}{2\sigma^2}} \\ & \Leftrightarrow
\underset{w}{argmin}\sum_{i=1}{\log {\sqrt {2\pi}\sigma} + \frac{||w^T \cdot x_i - y_i||^2}{2\sigma^2}} \\ & \Leftrightarrow
\underset{w}{argmin}||w^T \cdot x_i - y_i||^2
\end{aligned}
$$
​		最小二乘法是相当于用极大似然估计求噪声为高斯分布的的线性模型。
