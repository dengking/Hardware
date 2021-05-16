# Cache coherence

## Consistency model、multiple model

可以使用consistency model来进行分析。

可以使用multiple model来进行分析。

## 讲述cache coherence的文章

1、"cppreference [std::memory_order](https://en.cppreference.com/w/cpp/atomic/memory_order) # Modification order" 段

2、stackoverflow [Why it's termed read-modify-write but not read-write?](https://stackoverflow.com/questions/49452022/why-its-termed-read-modify-write-but-not-read-write) # [A](https://stackoverflow.com/a/49638936)

3、zhuanlan.zhihu [高并发编程--多处理器编程中的一致性问题(上)](https://zhuanlan.zhihu.com/p/48157076) 

讲得非常好

4、工程programming-language的`aristeia-C++and-the-Perils-of-Double-Checked-Locking` 章节

5、tutorialspoint [Cache Coherence and Synchronization](https://www.tutorialspoint.com/parallel_computer_architecture/parallel_computer_architecture_cache_coherence_synchronization.htm)

## wikipedia [Cache coherence](https://infogalactic.com/info/Cache_coherence) 

In [computer science](https://infogalactic.com/info/Computer_science), **cache coherence** is the **consistency** of shared resource data that ends up stored in multiple [local caches](https://infogalactic.com/info/Cache_(computing)). When clients in a system maintain [caches](https://infogalactic.com/info/CPU_cache) of a common memory resource, problems may arise with inconsistent data, which is particularly the case with CPUs in a [multiprocessing](https://infogalactic.com/info/Multiprocessing) system.

[![img](https://infogalactic.com/w/images/thumb/a/a1/Cache_Coherency_Generic.png/510px-Cache_Coherency_Generic.png)](https://infogalactic.com/info/File:Cache_Coherency_Generic.png)



In the illustration on the right, consider both the clients have a cached copy of a particular memory block from a previous read. Suppose the client on the bottom updates/changes that memory block, the client on the top could be left with an invalid cache of memory without any notification of the change. Cache coherence is intended to manage such conflicts by maintaining a coherent view of the data values in multiple caches.

> NOTE: 描述的是一个典型的inconsistent 的场景

### Overview

**Cache coherence** is the discipline which ensures that the changes in the values of **shared** operands (data) are propagated throughout the system in a timely fashion.[[1\]](https://en.wikipedia.org/wiki/Cache_coherence#cite_note-:1-1)

> NOTE: "propagate"是一个非常形象的描述

The following are the requirements for cache coherence:[[2\]](https://en.wikipedia.org/wiki/Cache_coherence#cite_note-:0-2)

> NOTE:下面这些requirement是为了保证什么？是为了保证coherence？

#### Write Propagation

Changes to the data in any cache must be propagated to other copies (of that cache line) in the peer caches.

#### Transaction Serialization

Reads/Writes to a single memory location must be seen by all processors in the same order.

> NOTE: 
>
> 一、下面会对Transaction Serialization进行说明，Transaction Serialization是非常重要的内容
>
> 二、上述两个requirement的含义是不同的，它们是必不可少的，在下面的"Definition"段依然是围绕它们而展开



Theoretically, coherence can be performed at the load/store [granularity](https://en.wikipedia.org/wiki/Granularity). However, in practice it is generally performed at the granularity of cache blocks.[[3\]](https://en.wikipedia.org/wiki/Cache_coherence#cite_note-:2-3)

> NOTE: unit

### Definition

> NOTE: 
>
> 一、原文比较长，下面是一些关键词:
>
> [consistency model](https://infogalactic.com/info/Consistency_model)
>
> [sequential consistency](https://infogalactic.com/info/Sequential_consistency) memory model
>
> [locality of reference](https://infogalactic.com/info/Locality_of_reference)
>
> 二、原文其实是从"Overview"段的提出的两个requirement入手来进行分析的
>
> 三、原文这一段中，花了很大篇幅来论述transaction serialization，其实之前就已经接触过transaction serialization了，只是没有定义这样的概念

The above conditions satisfy the Write Propagation criteria required for cache coherence. However, they are not sufficient as they do not satisfy the Transaction Serialization condition. To illustrate this better, consider the following example:

> NOTE: 
>
> 一、这一段是承上启下，从此就开始论述transaction serialization，省略了这一段后面紧跟的example。

Therefore, in order to satisfy **Transaction Serialization**, and hence achieve Cache Coherence, the following condition along with the previous two mentioned in this section must be met:

1、Writes to the same location must be sequenced. In other words, if location X received two different values A and B, in this order, from any two processors, the processors can never read location X as B and then read it as A. The location X must be seen with values A and B in that order.[[5\]](https://en.wanweibaike.com/wiki-Cache Coherence#cite_note-5)

> NOTE: 
>
> 一、上面这段话，从底层解释了 "write order to shared data different among different processor thread"，在实际CPU implementation，上述是可能出现的，参见 
>
> 1、aristeia [C++ and the Perils of Double-Checked Locking](https://www.aristeia.com/Papers/DDJ_Jul_Aug_2004_revised.pdf) 
>
> 其中就是详细说明了如果不遵循  **Transaction Serialization** 的危害
>
> 

#### Via the definition of [sequential consistency](https://en.wanweibaike.com/wiki-Sequential_consistency) memory model

The alternative definition of a coherent system is via the definition of [sequential consistency](https://en.wanweibaike.com/wiki-Sequential_consistency) memory model: "the cache coherent system must appear to execute all threads’ loads and stores to a *single* memory location in a **total order** that respects the program order of each thread".[[3\]](https://en.wanweibaike.com/wiki-Cache Coherence#cite_note-:2-3) Thus, the only difference between the cache coherent system and sequentially consistent system is in the number of address locations the definition talks about (single memory location for a cache coherent system, and all memory locations for a sequentially consistent system).

> NOTE: 
>
> 1、两个definition的本质是相同的，只是讨论的是不同范围的问题
>
> 

#### 不遵循transaction serialization

Rarely, but especially in algorithms, coherence can instead refer to the [locality of reference](https://en.wanweibaike.com/wiki-Locality_of_reference). Multiple copies of same data can exist in different cache simultaneously and if processors are allowed to update their own copies freely, an inconsistent view of memory can result.

> NOTE: 
>
> 1、上述"inconsistent view"是非常好的总结

### Coherency mechanisms

> NOTE: 

#### Snooping

Main article: [Bus snooping](https://en.wanweibaike.com/wiki-Bus_snooping)

> NOTE: 
>
> 1、"snoop"的意思"窥探"
>
> 2、这种是见得比较多一些的

First introduced in 1983,[[7\]](https://en.wanweibaike.com/wiki-Cache Coherence#cite_note-7) snooping is a process where the individual caches monitor address lines for accesses to memory locations that they have cached.[[4\]](https://en.wanweibaike.com/wiki-Cache Coherence#cite_note-:3-4) The *write-invalidate protocols* and *write-update protocols* make use of this mechanism.

#### Directory-based

*Main article:* [Directory-based cache coherence](https://en.wikipedia.org/wiki/Directory-based_cache_coherence)

In a **directory-based system**, the data being shared is placed in a **common directory** that maintains the coherence between caches. The directory acts as a filter through which the processor must ask permission to load an entry from the primary memory to its cache. When an entry is changed, the directory either updates or invalidates the other caches with that entry.

> NOTE: 将所有被shared的数据都放在一个共享的common directory中，由它来实现coherence between caches。processor需要由它的permission才能够cache一个数据。



[Distributed shared memory](https://en.wikipedia.org/wiki/Distributed_shared_memory) systems mimic these mechanisms in an attempt to maintain consistency between blocks of memory in loosely coupled systems.[[9\]](https://en.wikipedia.org/wiki/Cache_coherence#cite_note-9)

> NOTE: 同一思想在不同领域的应用



### Coherency protocols

> NOTE: 
>
> 1、这部分内容放到了`./Protocol`章节。

## How Cache Coherency Impacts Power Performance

参见 `Cache-performance-optimization` 章节。