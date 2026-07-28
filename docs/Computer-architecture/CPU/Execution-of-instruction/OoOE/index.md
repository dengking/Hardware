# Out-of-order execution

1、简单而言，OoOE是CPU为了performance，不按照In-order execution，即[instruction cycle](https://infogalactic.com/info/Instruction_cycle)，而是采取特殊的执行方式。

2、最最典型的Out-of-order execution是memory  reordering，在 `CPU-memory-access\Memory-ordering` 章节进行了总结。

## Wikipedia [Out-of-order execution](https://infogalactic.com/info/Out-of-order_execution)

In [computer engineering](https://infogalactic.com/info/Computer_engineering), **out-of-order execution** (or more formally **dynamic execution**), is a [paradigm](https://infogalactic.com/info/Paradigm) used in most high-performance [microprocessors](https://infogalactic.com/info/Microprocessor) to make use of [instruction cycles](https://infogalactic.com/info/Instruction_cycle) that would otherwise be wasted by a certain type of costly delay. In this paradigm, a processor executes instructions in an order governed by the availability of input data, rather than by their original order in a program.[[1\]](https://infogalactic.com/info/Out-of-order_execution#cite_note-1) In doing so, the processor can avoid being idle while waiting for the preceding instruction to complete to retrieve data for the next instruction in a program, processing instead the next instructions which are able to run immediately and independently.[[2\]](https://infogalactic.com/info/Out-of-order_execution#cite_note-2) It can be viewed as a hardware based [dynamic recompilation](https://infogalactic.com/info/Dynamic_recompilation) or [just-in-time compilation](https://infogalactic.com/info/Just-in-time_compilation) (JIT) to improve [instruction scheduling](https://infogalactic.com/info/Instruction_scheduling).

> NOTE: CPU的out-of-order execution是处于performance考虑的

### Basic concept

#### In-order processors

*Main article:* [Instruction cycle](https://infogalactic.com/info/Instruction_cycle)

#### Out-of-order processors

> NOTE: 比较复杂

The key concept of OoOE processing is to allow the processor to avoid a class of stalls that occur when the data needed to perform an operation are unavailable.

The benefit of OoOE processing grows as the [instruction pipeline](https://infogalactic.com/info/Instruction_pipeline) deepens and the speed difference between [main memory](https://infogalactic.com/info/Main_memory) (or [cache memory](https://infogalactic.com/info/Cache_memory)) and the processor widens. On modern machines, the processor runs many times faster than the memory, so during the time an in-order processor spends waiting for data to arrive, it could have processed a large number of instructions.

## 素材

### zhihu [如何理解 C++11 的六种 memory order？](https://www.zhihu.com/question/24301047) # [A](https://www.zhihu.com/question/24301047/answer/1193956492) 

> 啥？...还真的是这样。原因在于当代CPU内部也有指令重排。也就是说，CPU执行指令的顺序，也不见得是完全严格按照机器码的顺序。特别是，当代CPU的IPC（每时钟执行指令数）一般都远大于1，也就是所谓的**多发射**，很多命令都是同时执行的。比如，当代CPU当中（一个核心）一般会有2套以上的整数ALU（加法器），2套以上的浮点ALU（加法器），往往还有独立的乘法器，以及，独立的Load和Store执行器。Load和Store模块往往还有8个以上的队列，也就是可以同时进行8个以上内存地址（cache line）的读写交换。
>
> 是不是有些晕？简单来说，你可以理解当代CPU不仅是多核心，而且每个核心还是多任务（多指令）并行的。计算机课本上的那种一个时钟一条指令的，早就是老黄历了 （当然，宏观来看基本原理并没有改变）

### [Processor technologies](https://en.wikipedia.org/wiki/Processor_(computing)) # [Out-of-order](https://en.wikipedia.org/wiki/Out-of-order_execution)

其中总结了一些out of order的内容。

1、[Wide-issue](https://en.wikipedia.org/wiki/Wide-issue)

"wide issue"应该就是"multiple issue"，即多发射。

### umd.edu [Multiple Issue Processors I](https://www.cs.umd.edu/~meesh/411/CA-online/chapter/multiple-issue-processors-i/index.html)

> NOTE:
>
> 1、介绍地比较详细

**Types of Multiple Issue Processors:** There are basically two variations in multiple issue processors – Superscalar processors and VLIW (Very Long Instruction Word) processors. There are two types of superscalar processors that issue varying numbers of instructions per clock. They are

- **statically scheduled superscalars** that use in-order execution
- **dynamically scheduled superscalars** that use out-of-order execution