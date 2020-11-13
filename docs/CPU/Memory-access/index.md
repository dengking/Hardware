# 关于本章

本章讨论CPU和memory的交互。

## von Neumann bottleneck: "限制CPU速度的是从内存中读写数据"

记得大学时在学习**计算机组成原理**课程的时候，老师提出过重要的观点：

“限制CPU速度的是从内存中读写数据”

意思是CPU的ALU的运算速度是非常快的，相比之下从内存中读取是比较缓慢的，所以ALU常常需要等待，这应该是当代CPU设计时需要考虑的一个矛盾所在，各种缓解这个矛盾的技术不断出现，比如: 

1) 在Book-计算机组成原理-科学出版社的5.1.3CPU中的主要寄存器章节中所描述的**数据缓冲寄存器（DR）**的作用：补偿CPU和内存、外围设备之间在操作速度上的差别。

本章主要介绍CPU访问memory相关的内容，读者记住上面的这个观点，应该能够有助于理解使用一些技术的目的所在。

> NOTE: 这其实是 [von Neumann bottleneck](https://en.wikipedia.org/wiki/Von_Neumann_architecture#Von_Neumann_bottleneck)  ，参见`Computer-architecture\Von-Neumann-architecture`章节。

## Memory access

processor和memory之间的数据传输是processor的主要活动之一，本章主要对与此相关的内容进行整理，下面是涉及到的一些内容。

### [Memory–processor transfer](https://en.wikipedia.org/wiki/Word_(computer_architecture)#Uses_of_words)

When the processor reads from the memory subsystem into a register or writes a register's value to memory, the amount of data transferred is often a **word**. Historically, this amount of bits which could be transferred in one cycle was also called a *catena* in some environments. In simple memory subsystems, the word is transferred over the memory [data bus](https://en.wikipedia.org/wiki/Bus_(computing)), which typically has a width of a word or half-word. In memory subsystems that use [caches](https://en.wikipedia.org/wiki/CPU_cache), the word-sized transfer is the one between the processor and the first level of cache; at lower levels of the [memory hierarchy](https://en.wikipedia.org/wiki/Memory_hierarchy) larger transfers (which are a multiple of the word size) are normally used.

上面这段话给出了Memory–processor transfer的一个简单模型，这其中会涉及一系列问题，需要进行深入分析，本章后续章节会对此进行展开。





### [Register memory architecture](https://en.wikipedia.org/wiki/Register_memory_architecture)



### [Load–store architecture](https://en.wikipedia.org/wiki/Load%E2%80%93store_architecture)



> NOTE: Word、alignment、endian是programmer常常会遇到的关于CPU的一些内容，这将是本章的重点。

## Word

CPU的读写单位。

## Alignment



## Endian

关于memory的一个重要指标。



