---
layout: post
title: Federated Learning Experiment
category: 科研
tags: Meachine learning 论文 笔记
keywords: 
description: 台大李宏毅的课程笔记
---

## Lecture 2 Where does the error come from?

### Bias and Variance of Esstimator

样本的无偏估计
错误来自于 bias（期望值和真实值有多接近） 和 variance（点的分布有多开）

通过bias和variance可以判断是否underfitting还是overfitting。 bias大，需要重新设计model，增减更多的feature，设计更负载的模型。variance大的话需要增加训练的书籍量

## Lecture 3 Gradient Descent

如何把gradient descent做得更好

### Adaptive Descent

#### Adagrad

$$ w^{t+1} \leftarrow w^{t}-\frac{\eta^{t}}{\sigma^{t}} g^{t} $$

$$ \sigma^{t} 是过去所有微分值的root mean square $$ 

e.g
$$
\begin{array}{ll}{w^{1} \leftarrow w^{0}-\frac{\eta^{0}}{\sigma^{0}} g^{0}} & {\sigma^{0}=\sqrt{\left(g^{0}\right)^{2}}} \\ {w^{2} \leftarrow w^{1}-\frac{\eta^{1}}{\sigma^{1}} g^{1}} & {\sigma^{1}=\sqrt{\frac{1}{2}\left[\left(g^{0}\right)^{2}+\left(g^{1}\right)^{2}\right]}}\end{array}
$$

如何解释这个方法： 造成一个反差效果

#### Stochastic Gradient Descent

选一个点来计算loss来进行gradient Descent

#### Feature Scaling

调整参数分布，让分布相同

## Lecture 4 classification

### 数据构造

MNIST测试集中一共有60000张图片
0：5923
1：6742
2：5958
3：6131
4：5842
5：5421
6：5918
7：6265
8：5851
9：5949
