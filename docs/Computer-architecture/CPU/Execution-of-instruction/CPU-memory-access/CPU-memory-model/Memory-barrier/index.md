# Memory barrier

一、"barrier"的含义是"屏障"，它用于阻止、隔离

二、关于Memory barrier介绍地最好的文章是 preshing [Memory Barriers Are Like Source Control Operations](https://preshing.com/20120710/memory-barriers-are-like-source-control-operations/) ，其中有着非常好的描述。

三、需要注意的是，barrier和fence意思是相同的，关于此，参见:

a、stackoverflow [Is memory fence and memory barrier same?](https://stackoverflow.com/questions/59596654/is-memory-fence-and-memory-barrier-same)

b、wikipedia [Memory barrier](https://en.wikipedia.org/wiki/Memory_barrier)



## wikipedia [Memory barrier](https://en.wikipedia.org/wiki/Memory_barrier)

A **memory barrier**, also known as a **membar**, **memory fence** or **fence instruction**, is a type of [barrier](https://en.wikipedia.org/wiki/Barrier_(computer_science)) [instruction](https://en.wikipedia.org/wiki/Instruction_(computer_science)) that causes a [central processing unit](https://en.wikipedia.org/wiki/Central_processing_unit) (CPU) or [compiler](https://en.wikipedia.org/wiki/Compiler) to enforce an [ordering](https://en.wikipedia.org/wiki/Memory_ordering) constraint on [memory](https://en.wikipedia.org/wiki/Random-access_memory) operations issued before and after the **barrier instruction**. This typically means that operations issued prior to the barrier are guaranteed to be performed before operations issued after the barrier.

> NOTE: 
>
> 1、memory barrier 和 memory fence的意思是相同的
>
> 2、从本质上来说:
>
> CPU memory barrier instruction
>
> 3、上面这段话中 "causes a [central processing unit](https://en.wikipedia.org/wiki/Central_processing_unit) (CPU) or [compiler](https://en.wikipedia.org/wiki/Compiler) to enforce an [ordering](https://en.wikipedia.org/wiki/Memory_ordering) constraint on [memory](https://en.wikipedia.org/wiki/Random-access_memory) operations issued before and after the barrier instruction"、"This typically means that operations issued prior to the barrier are guaranteed to be performed before operations issued after the barrier"
>
> 总结地非常好，它描述了memory barrier的作用，其实概括起来也比较简单: memory barrier实施了"ordering constraint"，"ordering constraint"概念是在下面这段话中提出的

**Memory barriers** are necessary because most modern CPUs employ performance optimizations that can result in [out-of-order execution](https://en.wikipedia.org/wiki/Out-of-order_execution). This reordering of memory operations (**loads and stores**) normally goes unnoticed(未注意) within a single [thread of execution](https://en.wikipedia.org/wiki/Thread_(computer_science)), but can cause unpredictable behaviour in [concurrent programs](https://en.wikipedia.org/wiki/Concurrent_computing) and [device drivers](https://en.wikipedia.org/wiki/Device_driver) unless carefully controlled. The exact nature of an **ordering constraint** is hardware dependent and defined by the architecture's [memory ordering model](https://en.wikipedia.org/wiki/Memory_model_(programming)). Some architectures provide multiple barriers for enforcing different **ordering constraints**.

> NOTE: 
>
> 一、memory barrier应该这样来进行理解: 由于 [out-of-order execution](https://infogalactic.com/info/Out-of-order_execution) ，导致了很多问题，因此引入memory barrier来供programmer来解决这些问题，它的思路如下: 
>
> 1、ordering and computational，参见工程discrete的`Relation-structure-computation\Make-it-computational`章节。
>
> 2、control theory: 由programmer来添加显式的控制从而使之有序，非常类似于 `tf.control_dependency`
>
> 3、instruction level barrier
>
> 关于 [out-of-order execution](https://infogalactic.com/info/Out-of-order_execution) ，参见工程hardware的`CPU\Execution-of-instruction\Out-of-order-execution`章节；
>
> 二、[Memory ordering model](https://en.wikipedia.org/wiki/Memory_model_(programming)) 所链接的文章主要是描述的programming language的memory model。
>
> 三、上面这段话中提出了"ordering constraint"概念，非常精辟地概括了memory barrier的本质

Memory barriers are typically used when implementing low-level [machine code](https://en.wikipedia.org/wiki/Machine_code) that operates on memory shared by multiple devices. Such code includes [synchronization](https://en.wikipedia.org/wiki/Synchronization_(computer_science)) primitives and [lock-free](https://en.wikipedia.org/wiki/Non-blocking_synchronization) data structures on [multiprocessor](https://en.wikipedia.org/wiki/Multiprocessing) systems, and device drivers that communicate with [computer hardware](https://en.wikipedia.org/wiki/Personal_computer_hardware).

> NOTE: concurrent programming中的很多内容都是依赖于memory barrier的:
>
> 1 [synchronization](https://en.wikipedia.org/wiki/Synchronization_(computer_science)) primitives
>
> 2 [lock-free](https://en.wikipedia.org/wiki/Non-blocking_synchronization) data structures on [multiprocessor](https://en.wikipedia.org/wiki/Multiprocessing) systems
>
> 3 device drivers that communicate with [computer hardware](https://en.wikipedia.org/wiki/Personal_computer_hardware)
>
> 

### Example

> NOTE: 
>
> 一、这段内容非常好

### Multithreaded programming and memory visibility

> NOTE: 

Each API or programming environment in principle has its own high-level memory model that defines its memory visibility semantics. Although programmers do not usually need to use memory barriers in such high level environments, it is important to understand their memory visibility semantics, to the extent possible. Such understanding is not necessarily easy to achieve because memory visibility semantics are not always consistently specified or documented.

> NOTE: 
>
> 一、programming language的memory model需要对 "memory visibility semantic" 进行详细的定义，典型的例子就是C++ memory model、Java memory model

## stackoverflow [What is a memory fence?](https://stackoverflow.com/questions/286629/what-is-a-memory-fence)



### [A](https://stackoverflow.com/a/286705)

For performance gains modern CPUs often execute instructions out of order to make maximum use of the available silicon (including memory read/writes). Because the hardware enforces **instructions integrity** you never notice this in a **single thread of execution**. However for multiple threads or environments with volatile memory (memory mapped I/O for example) this can lead to unpredictable behavior.

> NOTE: 
>
> 一、上面提出了 "**instructions integrity**" 概念，它意味着 "you never notice this in a **single thread of execution**"

A memory fence/barrier is a class of **instructions** that mean memory read/writes occur in the order you expect. For example a 'full fence' means all read/writes before the fence are comitted before those after the fence.

> NOTE: 
>
> 其实就是"ordering constrain"

Note memory fences are a hardware concept. In higher level languages we are used to dealing with mutexes and semaphores - these may well be implemented using memory fences at the low level and explicit use of memory barriers are not necessary. Use of memory barriers requires a careful study of the hardware architecture and more commonly found in device drivers than application code.

The CPU reordering is different from compiler optimisations - although the artefacts can be similar. You need to take separate measures to stop the compiler reordering your instructions if that may cause undesirable behaviour (e.g. use of the volatile keyword in C).