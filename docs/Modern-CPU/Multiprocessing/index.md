# Multiprocessing

在当今CPU朝着parallel scaling方向的发展的情况下，我们需要思考CPU的结构，而Multiprocessing则是对这个话题的讨论。

## Wikipedia [Multiprocessing](https://infogalactic.com/info/Multiprocessing)

> NOTE: 可以使用multiple model来描述Multiprocessing ，参见工程parallel-computing的`Model\Multiple-model`章节，因此，在Multiprocessing，就会面临multiple model中描述的所有的问题。

**Multiprocessing** is the use of two or more [central processing units](https://infogalactic.com/info/CPU) (CPUs) within a single computer system.[[1\]](https://infogalactic.com/info/Multiprocessing#cite_note-Rajagopal1999-1)[[2\]](https://infogalactic.com/info/Multiprocessing#cite_note-EbbersKettner2012-2) The term also refers to the ability of a system to support more than one processor and/or the ability to allocate tasks between them.[[3\]](https://infogalactic.com/info/Multiprocessing#cite_note-3) There are many variations on this basic theme, and the definition of multiprocessing can vary with context, mostly as a function of how CPUs are defined ([multiple cores](https://infogalactic.com/info/Multi-core_(computing)) on one [die](https://infogalactic.com/info/Die_(integrated_circuit)), multiple dies in one [package](https://infogalactic.com/info/Chip_carrier), multiple packages in one [system unit](https://infogalactic.com/info/System_unit), etc.).

### Key topics

> NOTE: 这一段对Multiprocessing中关键topic的总结是非常好的。

#### Processor symmetry

> NOTE: "symmetry"的意思是"对称"。

Systems that treat all CPUs equally are called [symmetric multiprocessing](https://infogalactic.com/info/Symmetric_multiprocessing) (SMP) systems. In systems where all CPUs are not equal, system resources may be divided in a number of ways, including [asymmetric multiprocessing](https://infogalactic.com/info/Asymmetric_multiprocessing) (ASMP), [non-uniform memory access](https://infogalactic.com/info/Non-uniform_memory_access) (NUMA) multiprocessing, and [clustered](https://infogalactic.com/info/Computer_cluster) multiprocessing.

> NOTE: SMP、NUMA是我们经常遇到的。

#### Instruction and data streams

> NOTE: 暂时没有遇到相关的内容

#### Processor coupling

##### Tightly coupled multiprocessor system

##### Loosely coupled multiprocessor system

*Main article:* [shared nothing architecture](https://infogalactic.com/info/Shared_nothing_architecture)

#### Multiprocessor Communication Architecture

> NOTE:  multiple model中的典型问题

##### Message passing

Separate address space for each processor.

processors communicate via message passing.

processors provide local message queue memories.

focus attention on costly non-local operations.

##### Shared memory

Processors communicate with shared address space

Processors communicate by memory read/write

Easy on small-scale machines

Lower latency

SMP or NUMA architecture

### Flynn's taxonomy

|                           |         Single instruction stream          |        Multiple instruction streams        |               Single program               | Multiple programs                          |
| :-----------------------: | :----------------------------------------: | :----------------------------------------: | :----------------------------------------: | ------------------------------------------ |
|  **Single data stream**   | [SISD](https://infogalactic.com/info/SISD) | [MISD](https://infogalactic.com/info/MISD) |                                            |                                            |
| **Multiple data streams** | [SIMD](https://infogalactic.com/info/SIMD) | [MIMD](https://infogalactic.com/info/MIMD) | [SPMD](https://infogalactic.com/info/SPMD) | [MPMD](https://infogalactic.com/info/MPMD) |

> NOTE: **data stream**和**instruction stream**