---
layout: post
title: Secure Agre'ga'ti
category: 科研
tags: 论文 心得
keywords:
description:
---

### work based on Multiple non-colluding server

+ noninteractive
+ the computation among the server is very light-weight
+ allow input validated
+ Araki et.al presented a generic three-party computation protocol that achieves very high throughput

### Generic SMC

+ Yao's protocol not directly extend to hundreds users
+ based on secret-sharing is communication cost

### Worked based on Dining Cryptographers Networks

But previous DC-nets constructions share the flaw that, if even one user aborts the protocol before sending its message, the protocol must be restarted from scratch, which can be very expensive [36].
一个用户在消息发送前终止协议，需要重新发起协议

### Works based on Pairwise Additive Masking

the most closely related to google‘s scheme

the problem is also their recoveryphase is brittle

### Schemes based on (Threshold) Homomorphic Encryption

但计算成本高或需要额外的信任假设。 例如，基于Paillier的方案需要昂贵的生成阈值解密密钥集，其必须由可信第三方生成和分发，或者使用昂贵的协议在线生成。
