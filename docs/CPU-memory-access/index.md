# 关于本章

本章讨论CPU和memory的交互，它是CPU的核心activity。

## Von Neumann bottleneck: "限制CPU速度的是从内存中读写数据"

参见 `Computer-architecture\Von-Neumann-architecture`章节

## Memory access

processor和memory之间的数据传输是processor的主要活动之一，本章主要对与此相关的内容进行整理，下面是涉及到的一些内容。

### wikipedia [Memory–processor transfer](https://en.wikipedia.org/wiki/Word_(computer_architecture)#Uses_of_words)

When the processor reads from the memory subsystem into a register or writes a register's value to memory, the amount of data transferred is often a **word**. Historically, this amount of bits which could be transferred in one cycle was also called a *catena* in some environments. In simple memory subsystems, the word is transferred over the memory [data bus](https://en.wikipedia.org/wiki/Bus_(computing)), which typically has a width of a word or half-word. In memory subsystems that use [caches](https://en.wikipedia.org/wiki/CPU_cache), the word-sized transfer is the one between the processor and the first level of cache; at lower levels of the [memory hierarchy](https://en.wikipedia.org/wiki/Memory_hierarchy) larger transfers (which are a multiple of the word size) are normally used.

> NOTE: 上面这段话给出了Memory–processor transfer的一个简单模型，这其中会涉及一系列问题，需要进行深入分析，本章后续章节会对此进行展开。

### Instruction-set-architectures

在 `CPU\Instruction-set-architectures` 章节介绍了两种基于memory access的architecture: 

1、[Register memory architecture](https://en.wikipedia.org/wiki/Register_memory_architecture)

2、[Load–store architecture](https://en.wikipedia.org/wiki/Load%E2%80%93store_architecture)



## Operation on memory access

无论多么复杂，memory access最终都可以归纳为两种operation:

1、read

2、write

在hardware中，一般load、store来表示read、write。

## Memory-consistency model

CPU的consistency model是非常重要的一个内容，尤其是对于支持multiprocessing的CPU。这是我在阅读Wikipedia [Memory ordering](https://infogalactic.com/info/Memory_ordering)时，其中有相关描述:

> There are several memory-consistency models for [SMP](https://infogalactic.com/info/Symmetric_multiprocessing) systems





> NOTE: Word、alignment、endian是programmer常常会遇到的关于CPU的一些内容。

## Word

CPU memory access(读/写)单位。

## Alignment

CPU memory access(读/写) boundary。

非常重要的一个概念，它解释了非常多的内容。

## Endian

关于memory的一个重要指标。



