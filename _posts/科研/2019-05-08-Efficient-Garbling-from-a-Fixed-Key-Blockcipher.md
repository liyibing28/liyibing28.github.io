---
layout: post
title: Efficient Garbling from a Fixed-Key Blockcipher
category: 科研
tags: 密码学 安全多方计算 论文 笔记
keywords: 
description:
---

## Info

**Author:** Mihir Bellare, Viet Tung Hoang, Sriram Keelveedhi, Phillip Rogaway
**Affiliation:** University of California
**Year:** 2013

1. 这篇论文最主要的创意是什么?  
   1. Advocate schemes based on fixed-key AES as the best route to highly efficient circuit-garbling.
   2. This paper shows how to construct and evaluate GCs at unprecedented speeds.
2. 作者在这些创意的思想上，具体做了哪些工作？
   1. theoretical side, justify the security of these methods in the random-permutation model, where parties have access to a public random permutation.
   2. On the practical side, provide the JustGarble system
   3. We describe garbling schemes that evaluate a gate using a single call to a fixed permutation, which can be instantiated by fixed-key AES.
   4. We describe garbling schemes that evaluate a gate using a single call to a fixed permutation, which can be instantiated by fixed-key AES.
3. 这些创意在应用上有什么好处?
4. 这些创意和应用上的好处是在哪些条件下才能成立?(假设)  
   1. The security of these methods in the random-permutation model, where parties have access to a public random permutation.
   2. Starting point is the work of BHR
   > M. Bellare, V. Hoang, and P. Rogaway. Foundations of garbled circuits. In ACM Computer and Communications Security (CCS’12). ACM, 2012. Full version as ePrint Archive, Report 2012/265, May, 2012.
5. 这篇论文最主要的缺点或局限是什么?  
6. 这些缺点或局限在应用上有什么坏处?  
7. 这些缺点和应用上的坏处是因为哪些因素而引入的? (If the paper has any disadvantages, what are the causes for them)  
8. 如何避免这个问题(How to avoid such shortages in your proposed approach?)
9. 如果不能避免，是什么原因造成的(If they are unavoidable, what are the reasons for?)
10. 你建议学长学弟什么时候参考这篇论文的哪些部分(点子)?