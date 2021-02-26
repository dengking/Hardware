# Memory barrier



## Wikipedia [Memory barrier](https://en.wikipedia.org/wiki/Memory_barrier)

A **memory barrier**, also known as a **membar**, **memory fence** or **fence instruction**, is a type of [barrier](https://en.wikipedia.org/wiki/Barrier_(computer_science)) [instruction](https://en.wikipedia.org/wiki/Instruction_(computer_science)) that causes a [central processing unit](https://en.wikipedia.org/wiki/Central_processing_unit) (CPU) or [compiler](https://en.wikipedia.org/wiki/Compiler) to enforce an [ordering](https://en.wikipedia.org/wiki/Memory_ordering) constraint on [memory](https://en.wikipedia.org/wiki/Random-access_memory) operations issued before and after the barrier instruction. This typically means that operations issued prior to the barrier are guaranteed to be performed before operations issued after the barrier.

**Memory barriers** are necessary because most modern CPUs employ performance optimizations that can result in [out-of-order execution](https://en.wikipedia.org/wiki/Out-of-order_execution). This reordering of memory operations (**loads and stores**) normally goes unnoticed(未注意) within a single [thread of execution](https://en.wikipedia.org/wiki/Thread_(computer_science)), but can cause unpredictable behaviour in [concurrent programs](https://en.wikipedia.org/wiki/Concurrent_computing) and [device drivers](https://en.wikipedia.org/wiki/Device_driver) unless carefully controlled. The exact nature of an **ordering constraint** is hardware dependent and defined by the architecture's [memory ordering model](https://en.wikipedia.org/wiki/Memory_model_(programming)). Some architectures provide multiple barriers for enforcing different ordering constraints.

> NOTE: memory barrier应该这样来进行理解: 由于 [out-of-order execution](https://infogalactic.com/info/Out-of-order_execution) ，导致了很多问题，因此引入memory barrier来供programmer来解决这些问题，它的思路如下: 
>
> 1) ordering and computational，参见工程discrete的`Relation-structure-computation\Make-it-computational`章节。
>
> 2) control theory: 由programmer来添加显式的控制从而使之有序，非常类似于 `tf.control_dependency`
>
> 3) instruction level barrier
>
> 关于 [out-of-order execution](https://infogalactic.com/info/Out-of-order_execution) ，参见工程hardware的`CPU\Execution-of-instruction\Out-of-order-execution`章节；
>
> [Memory ordering model](https://en.wikipedia.org/wiki/Memory_model_(programming)) 所链接的文章重要是描述的programming language的memory model。

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

### An illustrative example



## stackoverflow [What is a memory fence?](https://stackoverflow.com/questions/286629/what-is-a-memory-fence)



### [A](https://stackoverflow.com/a/286705)

For performance gains modern CPUs often execute instructions out of order to make maximum use of the available silicon (including memory read/writes). Because the hardware enforces **instructions integrity** you never notice this in a **single thread of execution**. However for multiple threads or environments with volatile memory (memory mapped I/O for example) this can lead to unpredictable behavior.

A memory fence/barrier is a class of **instructions** that mean memory read/writes occur in the order you expect. For example a 'full fence' means all read/writes before the fence are comitted before those after the fence.

Note memory fences are a hardware concept. In higher level languages we are used to dealing with mutexes and semaphores - these may well be implemented using memory fences at the low level and explicit use of memory barriers are not necessary. Use of memory barriers requires a careful study of the hardware architecture and more commonly found in device drivers than application code.

The CPU reordering is different from compiler optimisations - although the artefacts can be similar. You need to take separate measures to stop the compiler reordering your instructions if that may cause undesirable behaviour (e.g. use of the volatile keyword in C).