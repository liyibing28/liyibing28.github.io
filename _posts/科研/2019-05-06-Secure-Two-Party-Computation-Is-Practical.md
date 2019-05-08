---
layout: post
title: Secure Two-Party Computation Is Practical
category: 科研
tags: 密码学 安全多方计算 论文 笔记
keywords: 
description:
---

## Info

**Author:** Benny Pinkas, Thomas Schneider, Nigel P. Smart, and Stephen C. Williams  
**Affiliation:** University of Haifa, Ruhr-University Bochum, University of Bristol  
**From:** ASIACRYPT  
**Years:** 2009  

## Introduction

1. 这篇论文最主要的创意是什么?  
   1. Improve on the implementation of 22 in a number of ways
      > Lindell, Y., Pinkas, B., Smart, N.P.: Implementing two-party computation effi- ciently with security against malicious adversaries. In: Ostrovsky, R., De Prisco, R., Visconti, I. (eds.) SCN 2008. LNCS, vol. 5229, pp. 2–20. Springer, Heidelberg (2008)
   2. 证明两方计算对于malicious安全是可行的，并且实验确定了我们优化过程中的性能瓶颈
   3. Improve the communication cost for transmitting the circuits between the parties. However, our improvement make a marginal impact on computational costs
   4. 对于三种模型都有涉及
2. 这些创意在应用上有什么好处?  
3. 这些创意和应用上的好处是在哪些条件下才能成立?(假设)  
   1. focus on the contruction of Lindell and Pinkas which is efficient and provides fully simulatable security according to the definition of canetti
    > Lindell, Y., Pinkas, B.: An efficient protocol for secure two-party computation in the presence of malicious adversaries. In: Naor, M. (ed.) EUROCRYPT 2007. LNCS, vol. 4515, pp. 52–78. Springer, Heidelberg (2007)
4. 作者在这些创意的思想上，具体做了哪些工作？
5. 这篇论文最主要的缺点或局限是什么?  
6. 这些缺点或局限在应用上有什么坏处?  
7. 这些缺点和应用上的坏处是因为哪些因素而引入的?If the paper has any disadvantages, what are the causes for them  
8. 如何避免这个问题(How to avoid such shortages in your proposed approach?)
9. 如果不能避免，是什么原因造成的(If they are unavoidable, what are the reasons for?)
10. 你建议学长学弟什么时候参考这篇论文的哪些部分(点子)?
11. 这篇论文的关注点是什么
   1. focus on implementation of secure computation