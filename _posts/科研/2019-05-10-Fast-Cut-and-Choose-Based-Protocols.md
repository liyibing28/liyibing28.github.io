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
   1. 采用了新的cut-and-choose技术
   2. 并且在达到同样安全性的情况下，将GC从128减少到40, 极大提高了效率
   3. 为了作弊，所有评估的电路必须是不正确的，而不仅仅是以前工作中的大多数不正确就行了。in order to cheat, all of the evaluated circuits must be incorrect, and not just the majority as in previous works.
2. 作者在这些创意的思想上，具体做了哪些工作？
   1. 
3. 这些创意在应用上有什么好处?  
   1. the method of cut-and-choose on garbled circuits is still the most efficient way of achieving security in the presence of covert and mali- cious adversaries.
4. 这些创意和应用上的好处是在哪些条件下才能成立?(假设)  
   1. 构建在convert模型之上(where the adversary may behave maliciously but is guaranteed to be caught with probability ? if it does)
5. 这篇论文最主要的缺点或局限是什么?  
   1. We stress that the bottleneck in protocols of this type is the computation and communication of the s garbled circuits.（s个GC的通信和计算开销）GC类型的论文的最大开销
6. 这些缺点或局限在应用上有什么坏处?  
   1. 
7. 这些缺点和应用上的坏处是因为哪些因素而引入的? (If the paper has any disadvantages, what are the causes for them)
   1. 
8. 如何避免这个问题(How to avoid such shortages in your proposed approach?)
   1. 
9. 如果不能避免，是什么原因造成的(If they are unavoidable, what are the reasons for?)
   1. 
10. 你建议学长学弟什么时候参考这篇论文的哪些部分(点子)?

## 论文细节

### what is cut-and-choose technique？

Protocols for cut-and-choose on garbled circuits [20,21,29,25] all work in the following way. Party $P_1$ constructs a large number of garbled circuits and sends them to party $P_2$. Party $P_2$ then chooses a subset of the circuits which are opened and checked. If all of these circuits are correct, then the remaining circuits are evaluated as in Yao’s protocol [30], and $P_2$ takes the majority output value as the output.

### Why does $P_2$ need take the majority output value as the output？

We stress that it is not possible for P2 to abort in case it receives different outputs in different circuits, even though in such a case it knows that P1cheated, because this opens the door to the following attack.A malicious P1can construct a single incorrect circuit that computes the following function: if the first bit of P2’s input equals 0 then output random garbage; else compute the correct function. Now, if this circuit is not opened (which happens with probability 1/2) and if the first bit of P2’s input equals 0, then P2 will receive a different output in this circuit and in the others. In contrast, if the first bit of P2’s input equals 1 then it always receives the same output in all circuits.  
Thus, if the protocol instructs P2to abort if it receives different outputs, then P1will learn the first bit of P2’s input (based on whether or not P2aborts).

### What's a selective input attack

 Selective input attack whereby $P_1$ provides correct garbled inputs only for a subset of the possible inputs of $P_2$ must be prevented (since otherwise $P_2$ will abort if its input is not in the subset because it cannot compute any circuit in this case, and thus $P_1$ will learn something about $P_2$’s input based on whether or not it aborts).

### Modified Batch Single-Chioce Cut-and-Choose OT

It is used to make sure the subset of indices for which the receiver obtains both pairs is the same in all transfers.

有欺骗的时候，获取两个值，没有欺骗的时候获取一个值（类似于常规的OT传输）

modified在于 here the receiver can obtain both values in an unknown number of transfers, as it desires.  
Therefore we need to introduce a mechanism enabling the receiver to prove to the sender in which transfers it did not receive both values, in a way that it cannot cheat.

### what's the main idea of paper 21

In paper 21, it was shown that when s circuits are sent and half of them are opened, the probability that P1can cheat is at most 2−0.311s.

### Analysis of protocol

Step | $p_1$ |  | $p_2$ |  |
:-: | :-: | :-: | :-: | :-:
1 | $a_1^0$ $a_1^1$...$a_l^0$$a_l^1$ <br> $r_1...r_s$ <br> $a_1^0$ $b_1^1$...$b_m^0$$b_m^1$ <br> $w_1...w_l$ <br> $w_{ij}$ <br> $k_{ij}^b$ <br> $w_i^‘$ |  |  | |
2 | |  | iii | 000|

## Some tips

+ 安全的定义
  + 被攻破的概率 $2^{-40}$