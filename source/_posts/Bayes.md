---
title: Bayes
date: 2022-10-15 12:48:05
tags: ML
categories: Machine Learning
cover: https://raw.githubusercontent.com/pikeyang/iamage/master/img/202210151319725.png?token=AOH5QIHMT23O6H65YFIJSC3DJJBS2
---

## 概念

- $P(Hypothesis)$ 先验概率，根据常识或者历史数据的统计得出的预判概率。

- $P(Hypothesis | Evidence)$  后验概率，其中 $|$ 意味着  Hypothesis given Evidence （在什么条件下），后验概率由于已知结果（实际观测值Evidence），所以是由果溯因。贝叶斯即属于由果溯因。

- $P(Evidence | Hypothesis)$ 似然概率，其中 $|$意味着 Limited （限制），即在假设成立的情况下，我们观测到证据的概率

- 条件概率是表示一个事件发生后另一个事件发生的概率。
- 联合概率指的是事件同时发生的概率。



​		在如下公式中，$\theta$已知时，$P$为关于x的概率函数，$x$已知时，$P$是关于$\theta$的似然函数。
$$
P(x|\theta)
$$


## 贝叶斯公式

​		贝叶斯公式在描述一个事情发生后，我们对于一件证据的相信程度。

>​		考虑这样一个场景，吸烟致癌。我们定义A为“吸烟”，B为“患肺癌”。吸烟有可能导致人患肺癌，但是患肺癌并非一定是吸烟所致，那么求解一个肺癌患者（B已发生）有多大可能是吸烟（A）的。那么我们已知一个人（B已发生），电动车被碰到了（A发生）的概率。

​		对于上述场景，我们容易将待求表示为$P(A|B)$，直接求解该式难度较大。但是人群中吸烟的比例相对容易得到（调查数据），然后吸烟致癌的可能性也可以获得（研究吸烟致癌的科研数据）。同时不吸烟患肺癌的数据也是可获得的。

<img src="https://raw.githubusercontent.com/pikeyang/iamage/master/img/202210151354879.png?token=AOH5QICJR3VUZWKA2TN5IY3DJJFUC" alt="贝叶斯" style="zoom:10%;" />

​		吸烟且患肺癌为$P(B|A)P(A)$， 不吸烟患肺癌为$P(B|\lnot A)P(\lnot A)$。那么问题求解科研表示为如下形式。
$$
P(A|B) = \frac{P(B|A)P(A)}{P(B|A)P(A)+P(B|\lnot A)P(\lnot A)}
$$
​		在吸烟致癌问题中，我们假定了患癌症的情况分为吸烟和不吸烟，但是在实际中，一件事情的发生往往都是多种诱因，那么可以将贝叶斯公式推广，写为更一般的形式。
$$
P(H_i|E) = \frac{P(E|H_i)P(H_i)}{\sum_i{P(E|H_i)P(H_i)}}
\label{eq:bayes}
$$
​		其中$H_i$为$E$的各种诱因。

## 全概率公式

​		$\eqref{eq:bayes}$ 的分母即为全概率公式。
$$
P(E) = \sum_i{P(E|H_i)P(H_i)}
$$
​		事件$E$发生的所有诱因放到一起可以计算出事件$E$发生的概率。在上面吸烟致癌的例子中就是，人群中患肺癌的比例。
$$
\begin{aligned}
P(E=患癌症) &= P(患癌症|吸烟)P(吸烟) + P(患癌症|不吸烟)P(不吸烟) \\ &=
15\% \times 80\% + 10\% \times 85\%
\end{aligned}
$$

## 似然概率 先验概率 后验概率


$$
Posterior \ probability \propto Likelihood \propto Prior \ probability
$$
