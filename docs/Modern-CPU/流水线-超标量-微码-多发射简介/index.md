# CPU流水线、多发射、超标量、CPU微码 

这些术语，时长碰到，有必要梳理一下。



## zhihu [关于CPU流水线 多发射 超标量 CPU微码 之间 的关系和原理?](https://www.zhihu.com/question/66374524)

### [A](https://www.zhihu.com/question/66374524/answer/243527000)

> 作者：Sinaean Dean
> 链接：https://www.zhihu.com/question/66374524/answer/243527000
> 来源：知乎
> 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
>
> 

流水线是指一条指令的执行被切分成多个阶段，交由不同的有独立功能的逻辑部件去依次执行。就好比一个人也可以做一道菜，你也可以把做菜分为洗菜、切菜、炒菜三道工序，交给三个人依次执行。



多发射就是说你有多条流水线，这样你原来可以交给CPU一条指令，现在可以同时交给他两条或是三条指令。



超标量是说你的CPU核心中有两个或更多的ALU，甚至FPU用来做计算。在没有多发射的情况下超标量也是可以用的，比如说一个ALU在执行一个多时钟周期的任务时，可以把下一条指令交给令一个ALU。这就好比你有你的菜每样菜炒出来所需时间不一样，有的块有的慢，但洗菜和切菜的速度是一样的，那有时可能配两个炒菜的师傅更好一下。

## nyu.edu [Lecture 17: Multiple Issue: SIMD, EPIC, and superscalar](https://cs.nyu.edu/courses/fall10/V22.0436-001/lecture17.html)



## 分别介绍

### wikipedia [Superscalar processor](https://en.wikipedia.org/wiki/Superscalar_processor)

A **superscalar processor** is a [CPU](https://en.wikipedia.org/wiki/Central_processing_unit) that implements a form of [parallelism](https://en.wikipedia.org/wiki/Parallel_computer) called [instruction-level parallelism](https://en.wikipedia.org/wiki/Instruction-level_parallelism) within a single processor. 

> NOTE: 单个 processor 内的 的并行

### wikipedia [Instruction pipelining](https://en.wikipedia.org/wiki/Instruction_pipelining)

In [computer science](https://en.wikipedia.org/wiki/Computer_science), **instruction pipelining** is a technique for implementing [instruction-level parallelism](https://en.wikipedia.org/wiki/Instruction-level_parallelism) within a single processor. Pipelining attempts to keep every part of the processor busy with some instruction by dividing incoming [instructions](https://en.wikipedia.org/wiki/Machine_code) into a series of sequential steps (the eponymous "[pipeline](https://en.wikipedia.org/wiki/Pipeline_(computing))") performed by different [processor units](https://en.wikipedia.org/wiki/Central_processing_unit#Structure_and_implementation) with different parts of instructions processed in parallel.

> NOTE: 
>
> 1、这一段读完后，就觉得它和前面的[Superscalar processor](https://en.wikipedia.org/wiki/Superscalar_processor)说的是同一个意思

### wikipedia [Wide-issue](https://en.wikipedia.org/wiki/Wide-issue)

A **wide-issue** architecture is a computer processor that issues more than one [instruction per clock cycle](https://en.wikipedia.org/w/index.php?title=Instruction_per_clock_cycle&action=edit&redlink=1).

> NOTE: 
>
> 1、一个时钟周期内发射多条指令
>
> 2、这应该是multiple issue

### umd.edu [Multiple Issue Processors I](https://www.cs.umd.edu/~meesh/411/CA-online/chapter/multiple-issue-processors-i/index.html)

> NOTE:
>
> 1、介绍地比较详细

**Types of Multiple Issue Processors:** There are basically two variations in multiple issue processors – Superscalar processors and VLIW (Very Long Instruction Word) processors. There are two types of superscalar processors that issue varying numbers of instructions per clock. They are

- **statically scheduled superscalars** that use in-order execution
- **dynamically scheduled superscalars** that use out-of-order execution

## Superscalar vs Instruction pipelining

在 [Superscalar processor](https://en.wikipedia.org/wiki/Superscalar_processor) 中有这样的介绍:

> While a superscalar CPU is typically also [pipelined](https://en.wikipedia.org/wiki/Instruction_pipeline), superscalar and pipelining execution are considered different performance enhancement techniques. The former executes multiple instructions in parallel by using multiple **execution units**, whereas the latter executes multiple instructions in the same execution unit in parallel by dividing the execution unit into different phases.

1、Instruction pipelining 是 使用多个 "**execution units**"

2、Superscalar 是 每个 "**execution units**" 内部将instruction分为多个阶段，然后并行执行

关于此，在 zhihu [如何理解 C++11 的六种 memory order？](https://www.zhihu.com/question/24301047) # [A](https://www.zhihu.com/question/24301047/answer/85844428) 中，有这样的总结:

> 简单来说，你可以理解当代CPU不仅是多核心，而且每个核心还是多任务（多指令）并行的。计算机课本上的那种一个时钟一条指令的，早就是老黄历了 （当然，宏观来看基本原理并没有改变）