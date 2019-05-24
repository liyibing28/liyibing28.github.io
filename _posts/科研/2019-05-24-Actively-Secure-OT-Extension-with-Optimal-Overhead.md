---
layout: post
title: Actively Secure OT Extension with Optimal Overhead
category: 科研
tags: 安全多方计算 OT 密码学 论文
keywords: 
description: 
---

## Info

## Questions

1. 这篇论文最主要的创意是什么?
   1. We describe an actively secure OT extension protocol in the random oracle model with efficiency very close to the passively secure IKNP protocol of Ishai et al.
2. 作者在这些创意的思想上，具体做了哪些工作？
   1. We give the first practical, actively secure OT extension protocol requiring only κ base OTs, matching the efficiency of the passively secure IKNP extension in this respect.
3. 这些创意在应用上有什么好处?
   1. 实现了actively secure和passive secure
4. 这些创意和应用上的好处是在哪些条件下才能成立?(假设)  
   1. our protocol requires 2 finite field (of size κ) operations per extended OT, plus a small communica- tion overhead of O(κ) bits in a constant number of rounds, independent of the number of OTs being performed,
   2. 安全性在random Oracle model上成立
5. 这篇论文最主要的缺点或局限是什么?  
6. 这些缺点或局限在应用上有什么坏处?  
7. 这些缺点和应用上的坏处是因为哪些因素而引入的? (If the paper has any disadvantages, what are the causes for them)
8. 如何避免这个问题(How to avoid such shortages in your proposed approach?)
9. 如果不能避免，是什么原因造成的(If they are unavoidable, what are the reasons for?)
10. 你建议学长学弟什么时候参考这篇论文的哪些部分(点子)?

## Papers

### 为什么要使用OT extension技术

It is very unlikely that OT is possible without the use of public-key cryptography, so all OT constructions have quite a high cost when used in a practical context.  
公钥密码学的成本太高,自然想到  
That is, starting with a small number of ‘base OTs’, create many more OTs with only symmetric primitives, some- what analogous to the use of hybrid encryption to extend public key encryption.
类似于采用混合加密提高公钥密码学性能  

### random oracle model([随机预言模型][1])

随机预言机模型通常是现实中哈希函数的理想化替身，其起源于把哈希函数看伪随机的函数的思想。

ROM理解为完美的散列函数具有以下性质：

（1）一致性：相同的输入必有相同的输出  
（2）可计算性：输出的计算可在多项式时间内完成  
（3）均匀分布性：预言机的输出在取值空间内均匀分布，无碰撞  

在随机预言机模型中，假定敌手不会利用散列函数的弱点来攻击密码学方案

随机预言机模型与标准模型关系：  
标准模型下敌手只受时间和计算能力的约束，而没有其他假设，可以将密码学方案归约到困难问题上，称方案具有标准模型下的可证明安全性。实际中很多方案在标准模型下建立安全性归约是比较困难的，因此为了降低证明的难度，往往在安全性归约过程中加入其他的假设条件，就变成了随机预言机模型。随机预言机模型过于理想化，但有利于安全性证明，方案中没有随机预言机模型会更好。

### 参考资料

[1]: https://blog.csdn.net/black_bearb/article/details/80886060