---
layout: post
title: Covert Security with Public Verifiability
category: 科研
tags: 密码学 安全多方计算 论文 笔记
keywords: 
description:
---

## Info

## Questions

1. 这篇论文最主要的创意是什么?
   1. propose a new PVC(public verified convert) protocol
   2. This protocol have some features relative to previous work as follow :
      1. Low overhead
      2. Small Certificates
      3. Simplicity
2. 作者在这些创意的思想上，具体做了哪些工作？
   1. address selective falture attacks
   2. use standard OT extension
      1. 以上的工作通过一个随机数来确定GC的所有执行来做到（为什么可以做到）
   3. use cut-and-choose approach
   4. evaluate one and check outher GC
   5. consider $P_B$ is malicious
3. 这些创意在应用上有什么好处?
4. 这些创意和应用上的好处是在哪些条件下才能成立?(假设)  
   1. we adapt the functionality such that only a malicious PAcan possibly cheat, as this is what is achieved by our protocol.
5. 这篇论文最主要的缺点或局限是什么?  
6. 这些缺点或局限在应用上有什么坏处?  
   1. 
7. 这些缺点和应用上的坏处是因为哪些因素而引入的? (If the paper has any disadvantages, what are the causes for them)
   1. 
8. 如何避免这个问题(How to avoid such shortages in your proposed approach?)
   1. 
9. 如果不能避免，是什么原因造成的(If they are unavoidable, what are the reasons for?)
   1. 
10. 你建议学长学弟什么时候参考这篇论文的哪些部分(点子)?
