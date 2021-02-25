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

## Wikipedia [Cache coherence](https://infogalactic.com/info/Cache_coherence)

In [computer science](https://infogalactic.com/info/Computer_science), **cache coherence** is the **consistency** of shared resource data that ends up stored in multiple [local caches](https://infogalactic.com/info/Cache_(computing)). When clients in a system maintain [caches](https://infogalactic.com/info/CPU_cache) of a common memory resource, problems may arise with inconsistent data, which is particularly the case with CPUs in a [multiprocessing](https://infogalactic.com/info/Multiprocessing) system.

[![img](https://infogalactic.com/w/images/thumb/a/a1/Cache_Coherency_Generic.png/510px-Cache_Coherency_Generic.png)](https://infogalactic.com/info/File:Cache_Coherency_Generic.png)



In the illustration on the right, consider both the clients have a cached copy of a particular memory block from a previous read. Suppose the client on the bottom updates/changes that memory block, the client on the top could be left with an invalid cache of memory without any notification of the change. Cache coherence is intended to manage such conflicts by maintaining a coherent view of the data values in multiple caches.

> NOTE: 描述的是一个典型的inconsistent 的场景

### Overview

**Cache coherence** is the discipline which ensures that the changes in the values of **shared** operands (data) are propagated throughout the system in a timely fashion.[[1\]](https://en.wikipedia.org/wiki/Cache_coherence#cite_note-:1-1)

> NOTE: "propagate"是一个非常形象的描述

The following are the requirements for cache coherence:[[2\]](https://en.wikipedia.org/wiki/Cache_coherence#cite_note-:0-2)

> NOTE:下面这些requirement是为了保证什么？是为了保证coherence？

-- Write Propagation

Changes to the data in any cache must be propagated to other copies (of that cache line) in the peer caches.

-- Transaction Serialization

Reads/Writes to a single memory location must be seen by all processors in the same order.

Theoretically, coherence can be performed at the load/store [granularity](https://en.wikipedia.org/wiki/Granularity). However, in practice it is generally performed at the granularity of cache blocks.[[3\]](https://en.wikipedia.org/wiki/Cache_coherence#cite_note-:2-3)

> NOTE: unit

### Definition

> NOTE: 原文比较长，下面是一些关键词:
>
> [consistency model](https://infogalactic.com/info/Consistency_model)
>
> [sequential consistency](https://infogalactic.com/info/Sequential_consistency) memory model
>
> [locality of reference](https://infogalactic.com/info/Locality_of_reference)

### Coherency mechanisms

> NOTE: 不容易理解

**Directory-based**

*Main article:* [Directory-based cache coherence](https://en.wikipedia.org/wiki/Directory-based_cache_coherence)

In a **directory-based system**, the data being shared is placed in a **common directory** that maintains the coherence between caches. The directory acts as a filter through which the processor must ask permission to load an entry from the primary memory to its cache. When an entry is changed, the directory either updates or invalidates the other caches with that entry.

> NOTE: 将所有被shared的数据都放在一个共享的common directory中，由它来实现coherence between caches。processor需要由它的permission才能够cache一个数据。

[Distributed shared memory](https://en.wikipedia.org/wiki/Distributed_shared_memory) systems mimic these mechanisms in an attempt to maintain consistency between blocks of memory in loosely coupled systems.[[9\]](https://en.wikipedia.org/wiki/Cache_coherence#cite_note-9)

> NOTE: 同一思想在不同领域的应用



### Coherency protocols

[Distributed shared memory](https://infogalactic.com/info/Distributed_shared_memory) systems employ *coherency protocols* to ensure the consistency between all caches, by maintaining [memory coherence](https://infogalactic.com/info/Memory_coherence) according to a specific [consistency model](https://infogalactic.com/info/Consistency_model). Older multiprocessors support the [sequential consistency](https://infogalactic.com/info/Sequential_consistency) model, while modern shared memory systems typically support the [release consistency](https://infogalactic.com/info/Release_consistency) or [weak consistency](https://infogalactic.com/info/Weak_consistency) models.



**Write propagation** in snoopy protocols can be implemented by either of the following methods:

> NOTE: 两种不同的方式

-- Write-invalidate

When a write operation is observed to a location that a cache has a copy of, the cache controller invalidates its own copy of the snooped memory location, which forces a read from main memory of the new value on its next access.[[4\]](https://en.wikipedia.org/wiki/Cache_coherence#cite_note-:3-4)

-- Write-update

When a write operation is observed to a location that a cache has a copy of, the cache controller updates its own copy of the snooped memory location with the new data.