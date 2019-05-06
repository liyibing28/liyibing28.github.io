---
layout: post
title: More Efficient Oblivious Transfer and Extensions for Faster Secure Computation
category: 科研
tags: 笔记 论文 安全多方计算 garbled-circuit 密码学
keywords: 
description: 这是ccs' 13上的一篇安全多方计算相关论文
---
## Info

**Author:** Gilad Asharov, Yehuda Lindell  ||  Thomas Schneider, Michael Zohner  
**Affliction:** Bar-Ilan University, Israel  ||  TU Darmstadt, Germany  
**From:** CCS  
**Year:** 2013

## Abstract

+ present optimizations and efficient implementations of OT and OT extensions in the **semi-honest model**.
+ propose a novel OT protocol with security in the standard model and improve OT extensions with respect to communication complexity, computation complexity, and scalability.
+ 为 Yao and Goldreich-Micali-Wigderson 等人的模型定制了协议，能降低复杂度
+ **experiment:**
  + securely compute a Levenshtein distance circuit with 1.29 billion AND gates at a rate of 1.2 million AND gates per second.
  + demonstrate the importance of correctly implementing OT within secure computation protocols by presenting an attack on the FastGC framework.

## Introduction

### Background

Exposing many problems, including privacy-preserving data mining, anonymous transactions, private database search, and many more.

Practical secure computation is feasiable but need to improve. Significant efficiency improve- ments can still be made, considerably broadening the tasks that can be solved using secure computation in practice.

**Oblivious transfer and extensions**  
the efficient instantiation of OT is of crucial importance as is evident in many recent works that focus on efficiency（关注OT协议的实例化)
> M. Naor and B. Pinkas. Efficient oblivious transfer protocols. In ACM-SIAM Symposium On Discrete Algorithms, SODA ’01, pages 448–457. Society for Industrial and Applied Mathematics, 2001.

This paper purposes the best known OT protocol. However, if thousands, millions or even billions of oblivious transfers need to be carried out, this will become prohibitively expensive.（当传输量大的时候，开销太大）

Therefore, some OT extensions are proposed. An OT extension protocol works by running a small number of OTs (say, 80 or 128) that are used as a base for obtaining many OTs via the use of cheap symmetric cryptographic operations only.

### Contributions and Outline

### Assumptions

1. semi-honest adversaries who follow the protocol, but may attempt to learn more than allowed via the protocol communication.

## Idea

+ Why is the assumption that consider semi-honest adversary reasonable？  
**Answer:**
  + For computations between hospitals or companies that trust each other but need to run a secure protocol because of legal restrictions and/or in order to prevent inadvertent leakage
  + Understanding the cost of semi-honest security is an important stepping stone to efficient malicious security.

+ 与云计算的应用相结合

## expression

+ the bulk of research on secure computation was theoretical in nature.