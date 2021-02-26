# Memory ordering

## wikipedia [Memory ordering](https://en.wikipedia.org/wiki/Memory_ordering)

**Memory ordering** describes the order of accesses to computer memory by a CPU. The term can refer either to the memory ordering generated by the [compiler](https://infogalactic.com/info/Compiler) during [compile time](https://infogalactic.com/info/Compile_time), or to the memory ordering generated by a CPU during [runtime](https://infogalactic.com/info/Run_time_(program_lifecycle_phase)).

> NOTE: compiler time和runtime

In modern [microprocessors](https://infogalactic.com/info/Microprocessor), memory ordering characterizes the CPUs ability to reorder memory operations - it is a type of [out-of-order execution](https://infogalactic.com/info/Out-of-order_execution). Memory reordering can be used to fully utilize the bus-bandwidth of different types of memory such as [caches](https://infogalactic.com/info/CPU_cache#Cache_entries) and [memory banks](https://infogalactic.com/info/Memory_bank).

> NOTE: CPU的一种优化方式

On most modern uniprocessors memory operations are **not** executed in the order specified by the program code.

> NOTE: 这是一个重要事实，可能与我们的直觉相违背

In single threaded programs all operations appear to have been executed in the order specified, with all out-of-order execution hidden to the programmer – however in multi-threaded environments (or when interfacing with other hardware via memory buses) this can lead to problems. 

> NOTE: ordering对programmer是透明的。multi-threaded environments中的一个典型问题就是使用 [Double-checked locking](https://infogalactic.com/info/Double-checked_locking) 来实现 [Singleton pattern](https://infogalactic.com/info/Singleton_pattern). 

To avoid problems [memory barriers](https://infogalactic.com/info/Memory_barrier) can be used in these cases.

### Compile-time memory ordering

The [compiler](https://infogalactic.com/info/Compiler) has some freedom to resort the order of operations during [compile time](https://infogalactic.com/info/Compile_time). However this can lead to problems if the order of memory accesses is of importance.

> NOTE: 关于[Memory barrier](https://infogalactic.com/info/Memory_barrier)的讨论放到了`Memory-barrier`章节。



### Runtime memory ordering

#### In symmetric multiprocessing (SMP) microprocessor systems

There are several **memory-consistency models** for [SMP](https://infogalactic.com/info/Symmetric_multiprocessing) systems:

1、Sequential consistency (all reads and all writes are in-order)

2、Relaxed consistency (some types of reordering are allowed)

- Loads can be reordered after loads (for better working of cache coherency, better scaling)
- Loads can be reordered after stores
- Stores can be reordered after stores
- Stores can be reordered after loads

> NOTE: four type memory ordering

3、Weak consistency (reads and writes are arbitrarily reordered, limited only by explicit [memory barriers](https://infogalactic.com/info/Memory_barrier))



> NOTE: 关于[Memory barrier](https://infogalactic.com/info/Memory_barrier)的讨论放到了`Memory-barrier`章节。







## Four type memory ordering and semantic

参考: 

1、zhihu [如何理解 C++11 的六种 memory order？](https://www.zhihu.com/question/24301047) # [A](https://www.zhihu.com/question/24301047/answer/85844428) 

2、preshing [Memory Barriers Are Like Source Control Operations](https://preshing.com/20120710/memory-barriers-are-like-source-control-operations/)

3、wikipedia [Memory ordering](https://en.wikipedia.org/wiki/Memory_ordering)

---



CPU的memory instruction有两种: load(read)、store(store)，因此可能的reordering只有如下四种组合: 

![img](https://preshing.com/images/barrier-types.png)

> 1、上述table是源自 preshing [Memory Barriers Are Like Source Control Operations](https://preshing.com/20120710/memory-barriers-are-like-source-control-operations/)。
>
> 2、上述四种memory reordering非常重要，后续很多关于memory的分析都是基于它的。

对于每一种reordering，CPU往往提供了对其进行控制的instruction，这就是memory barrier。提供memory barrier，programmer对memory ordering进行控制，从而实现相应的memory semantic，下面是对此的总结: 



### 从data dependency的角度来分析这四种ordering

1、load-load、store-store、load-store都没有data dependency，因此执行这种reordering，一般不会体现对program order的违背。

2、store-load 存在data dependency(写后，能够读到刚刚写入的值)，这体现在program order中，如果允许这种reordering，那么显然就违反了 program order，显然就违反了sequential consistency。

> NOTE: 
>
> 1、这段总结非常重要，它是理解sequential consistency的基础
>
> 2、关于这一段的内容，参见 :
>
> a、"preshing [Memory Barriers Are Like Source Control Operations](https://preshing.com/20120710/memory-barriers-are-like-source-control-operations/) # #StoreLoad"段
>
> b、[std::memory_order](https://en.cppreference.com/w/cpp/atomic/memory_order) # Sequentially-consistent ordering



### Reordering、memory barrier、memory semantic



**zhihu [如何理解 C++11 的六种 memory order？](https://www.zhihu.com/question/24301047) # [A](https://www.zhihu.com/question/24301047/answer/85844428)** :

**aquire语义**：load 之后的**读写操作**无法被重排至 load 之前。即 load-load, load-store 不能被重排。

**release语义**：store 之前的**读写操作**无法被重排至 store 之后。即 load-store, store-store 不能被重排。

> NOTE: 
>
> 1、可以看到，比较特殊的是**load-store** 



**sequential consistency**

上述四种reordering都是不允许的。



### 总结

| reordering               | 含义 | memory barrier/fence               |
| ------------------------ | ---- | ---------------------------------- |
| load-load(read-read)     |      | acquire semantic                   |
| store-store(write-write) |      | release semantic                   |
| load-store               |      | release semantic、acquire semantic |
| store-load               |      | sequential consistency             |

1、含义这一列省略了，参见 preshing [Memory Barriers Are Like Source Control Operations](https://preshing.com/20120710/memory-barriers-are-like-source-control-operations/)，其中有非常好的描述。

2、最后一列的含义是: 对于每一种reordering，都有对应的memory barrier来阻止它，添加了对应的memory barrier，就能够保证对应的semantic。



## Examples

在下面文章、章节中，讨论了Out of order execution 和 memory reordering，并给出了一些非常好的例子: 

1、"aristeia-C++and-the-Perils-of-Double-Checked-Locking" 章节

其中对此进行了非常好的介绍，这篇文章非常好

2、zhihu [如何理解 C++11 的六种 memory order？](https://www.zhihu.com/question/24301047) # [A](https://www.zhihu.com/question/24301047/answer/1193956492)

3、`Concurrent-computing\Expert-Jeff-Preshing\Lock-Free-Programming`

他的这个系列的文章非常好，可以作为guideline。

4、工程hardware的如下章节:

a、`CPU\Execution-of-instruction`

b、`CPU-memory-access\Memory-ordering`

5、`acmqueue-Shared-Variables-or-Memory-Models`

6、stackoverflow [How to understand acquire and release semantics?](https://stackoverflow.com/questions/24565540/how-to-understand-acquire-and-release-semantics) # [A](https://stackoverflow.com/a/9764313)

收录在工程programming language的`Release-acquire` 章节中，这篇文章非常好。

7、cppreference [std::memory_order](https://en.cppreference.com/w/cpp/atomic/memory_order) # Explanation # Relaxed ordering

其中给出了example，非常好。