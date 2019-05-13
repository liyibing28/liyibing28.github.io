---
layout: post
title: Fast Cut-and-Choose Based Protocols
category: 科研
tags: 密码学 安全多方计算 论文 笔记
keywords: 
description:
---

## Info

## Questions

1. 这篇论文最主要的创意是什么?
2. 作者在这些创意的思想上，具体做了哪些工作？
3. 这些创意在应用上有什么好处?  
4. 这些创意和应用上的好处是在哪些条件下才能成立?(假设)  
5. 这篇论文最主要的缺点或局限是什么?  
   1. We stress that the bottleneck in protocols of this type is the computation and communication of the s garbled circuits.（s个GC的通信和计算开销）GC类型的论文的最大开销
6. 这些缺点或局限在应用上有什么坏处?  
7. 这些缺点和应用上的坏处是因为哪些因素而引入的? (If the paper has any disadvantages, what are the causes for them)  
8. 如何避免这个问题(How to avoid such shortages in your proposed approach?)
9. 如果不能避免，是什么原因造成的(If they are unavoidable, what are the reasons for?)
10. 你建议学长学弟什么时候参考这篇论文的哪些部分(点子)?

## 论文细节

### what is cut-and-choose technique？

Protocols for cut-and-choose on garbled circuits [20,21,29,25] all work in the following way. Party $P_1$ constructs a large number of garbled circuits and sends them to party $P_2$. Party $P_2$ then chooses a subset of the circuits which are opened and checked. If all of these circuits are correct, then the remaining circuits are evaluated as in Yao’s protocol [30], and $P_2$ takes the majority output value as the output.

### Why does $P_2$ need take the majority output value as the output？

### What's a selective input attack

 Selective input attack whereby $P_1$ provides correct garbled inputs only for a subset of the possible inputs of $P_2$ must be prevented (since otherwise $P_2$ will abort if its input is not in the subset because it cannot compute any circuit in this case, and thus $P_1$ will learn something about $P_2$’s input based on whether or not it aborts).

## Some tips

+ 安全的定义
  + 被攻破的概率 $2^{-40}$