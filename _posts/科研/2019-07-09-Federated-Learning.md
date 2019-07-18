---
layout: post
title: Federated Learning
category: 科研
tags: FederatedLearning 联邦学习 论文 笔记
keywords: 
description:
---

## 调研

## 实验

### adaptive Federated learning in resource constrained edge computing systems

+ networked prototype system with 5 nodes
+ a simulated environment with the number of nodes varying from 5 to 500. The prototype system consists of three Raspberry Pi (version 3) devices and two laptop computers, which are all interconnected via Wi-Fi in an office building. This represents an edge computing environment where the computational capabilities of edge nodes are heterogeneous
+ we train each model for a fixed amount of time budget.
+ 模拟资源消耗服从高斯分布
+ baseline
  + Centralized gradient descent
  + Canonical federated learning approach 规范的联邦学习方法
  + Synchronous distributed gradient descent
+ 为了避免与损失计算相关的资源消耗，集中式梯度下降使用最后的模型参数w（T）（而不是wf）作为结果，因为w（T）的收敛可以在集中式情况下得到证实
+ SGD中 minibatch
+ The models include squared-SVM, linear regression, K-means, and deep convolutional neural networks (CNN).
+ 分布式配置
  + In Case 1, each data sample is randomly assigned to a node, thus each node has uniform (but not full) information.
  + In Case 2, all the data samples in each node have the same label.
  + Case 3, each node has the entire dataset (thus full information).
  + In Case 4, data samples with the first half of the labels are distributed to the first half of the nodes as in Case 1; the other samples are distributed to the second half of the nodes as in Case 2. 中和平衡和非平衡的情况

### 真实实验

### 模拟实验
