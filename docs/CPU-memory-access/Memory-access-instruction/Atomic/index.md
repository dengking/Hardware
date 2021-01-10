# 关于本章

本章讨论memory access 和 atomicity 的一些话题。

## 触发我总结这一章节的几个原因

1、preshing [Atomic vs. Non-Atomic Operations](https://preshing.com/20130618/atomic-vs-non-atomic-operations/) 这篇文章非常好，探寻了最最本质的原因

2、thread safety: 从指令级别来分析线程安全性

3、最小单位: 显然instruction是最小单位，因此一条instruction是atomic的

## Memory access unit and atomic

在 `./Memory-access-unit-and-atomic` 章节对这个问题进行总结。



## Atomic instruction

参见`./Atomic-instruction`章节。



## Application

non-blocking algorithm。



## Implementation

本章讲述的是通用规则，另外各个CPU的厂商的实现有所不同，参见:

1、Intel: `Instruction-set-architectures\Manufacturer\Intel\x86\Atomicity-on-x86`