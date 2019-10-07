---
layout: post
title: Federated Learning Experiment
category: 科研
tags: FederatedLearning 联邦学习 论文 笔记
keywords: 
description:
---

## TensorFlow的基本使用

### TensorFlow的特点

+ 使用图 (graph) 来表示计算任务.
+ 在被称之为 会话 (Session) 的上下文 (context) 中执行图.
+ 使用 tensor 表示数据.
+ 通过 变量 (Variable) 维护状态.
+ 使用 feed 和 fetch 可以为任意的操作(arbitrary operation) 赋值或者从其中获取数据.

### TensorFlow 计算图

TensorFlow 程序通常被组织成一个构建阶段和一个执行阶段. 在构建阶段, op 的执行步骤 被描述成一个图. 在执行阶段, 使用会话执行执行图中的 op.

#### 构建图

构建图的第一步, 是创建源 op (source op).源 op 不需要任何输入, 例如 常量 (Constant). 源 op 的输出被传递给其它 op 做运算. Python 库中, op 构造器的返回值代表被构造出的 op 的输出, 这些返回值可以传递给其它 op 构造器作为输入. TensorFlow Python 库有一个默认图 (default graph), op 构造器可以为其增加节点。这个默认图对许多程序来说已经足够用了。

#### 启动图

构造阶段完成后, 才能启动图。启动图的第一步是创建一个 Session 对象，如果无任何创建参数, 会话构造器将启动默认图。

Session 对象在使用完后需要关闭以释放资源. 除了显式调用 close 外, 也可以使用 "with" 代码块 来自动完成关闭动作.

### 变量

### Fetch

用于取回多个tensor

### Feed

```python
input1 = tf.placeholder(tf.types.float32)
input2 = tf.placeholder(tf.types.float32)
output = tf.mul(input1, input2)

with tf.Session() as sess:
  print sess.run([output], feed_dict={input1:[7.], input2:[2.]})
```

## Federated learning 教程 

