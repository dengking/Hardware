# Cache coherence



## Wikipedia [Cache coherence](https://infogalactic.com/info/Cache_coherence)

In [computer science](https://infogalactic.com/info/Computer_science), **cache coherence** is the consistency of shared resource data that ends up stored in multiple [local caches](https://infogalactic.com/info/Cache_(computing)). When clients in a system maintain [caches](https://infogalactic.com/info/CPU_cache) of a common memory resource, problems may arise with inconsistent data, which is particularly the case with CPUs in a [multiprocessing](https://infogalactic.com/info/Multiprocessing) system.

[![img](https://infogalactic.com/w/images/thumb/a/a1/Cache_Coherency_Generic.png/510px-Cache_Coherency_Generic.png)](https://infogalactic.com/info/File:Cache_Coherency_Generic.png)



An illustration showing multiple caches of some memory, which acts as a shared resource

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

In a directory-based system, the data being shared is placed in a **common directory** that maintains the coherence between caches. The directory acts as a filter through which the processor must ask permission to load an entry from the primary memory to its cache. When an entry is changed, the directory either updates or invalidates the other caches with that entry.

> NOTE: 将所有被shared的数据都放在一个共享的common directory中，由它来实现coherence between caches。processor需要由它的permission才能够cache一个数据。



### Coherency protocols

[Distributed shared memory](https://infogalactic.com/info/Distributed_shared_memory) systems employ *coherency protocols* to ensure the consistency between all caches, by maintaining [memory coherence](https://infogalactic.com/info/Memory_coherence) according to a specific [consistency model](https://infogalactic.com/info/Consistency_model). Older multiprocessors support the [sequential consistency](https://infogalactic.com/info/Sequential_consistency) model, while modern shared memory systems typically support the [release consistency](https://infogalactic.com/info/Release_consistency) or [weak consistency](https://infogalactic.com/info/Weak_consistency) models.