# Memory access unit and atomic

在 `../../Memory-alignment` 章节中，已经讨论了这个问题，本节对这个topic进行总结。

## Unit and atomic

Memory access unit是Word，因此，一次memory write、一次memory read是atomic的。

一个operation，如果它只需要执行一次memory write、一次memory read，那么它就是atomic的。一般，条件如下

1、aligned

2、length <= length of Word

> NOTE: 上述两个条件都是为了保证只进行一次memory access。
>
> 另外还需要结合如下知识来进行理解:
>
> 1、Memory processor transfer机制
>
> 2、alignment
>
> 对于 length < length of Word 的，典型的是bool，在 stackoverflow [is assignment operator '=' atomic?](https://stackoverflow.com/questions/8290768/is-assignment-operator-atomic)  中进行了讨论。

### Atomic operation and thread safe

在multiple-thread情况下，atomic operation不一定能够保证thread safe，反例包括:

1、stackoverflow [is assignment operator '=' atomic?](https://stackoverflow.com/questions/8290768/is-assignment-operator-atomic) 

在下面的"Assignment(=): bool"章节中进行了讨论



---

下面结合几种具体的情况来进行分析: 

### stackoverflow [Purpose of memory alignment](https://stackoverflow.com/questions/381244/purpose-of-memory-alignment) # [A](https://stackoverflow.com/a/381368) 

#### Atomicity

**The CPU can operate on an aligned word of memory atomically**, meaning that no other instruction can interrupt that operation. This is critical to the correct operation of many [lock-free data structures](http://kukuruku.co/hub/cpp/lock-free-data-structures-basics-atomicity-and-atomic-primitives) and other [concurrency](http://www.sciencedirect.com/science/article/pii/0304397588900965) paradigms.

> NOTE: 上面这段话可以这样理解: 
>
> 1、"unit and atomic": Memory processor transfer的unit是Word，因此"The CPU can operate on an aligned word of memory atomically"



## Assignment(=): pointer 



### Write to single aligned pointers are atomic on modern CPUs

这是我在阅读 wikipedia [Read-copy-update](http://en.wiki.sxisa.org/wiki/Read-copy-update) 时，其中提出的:

> In contrast, RCU-based updaters typically take advantage of the fact that writes to single aligned pointers are atomic on modern CPUs, allowing atomic insertion, removal, and replacement of data in a linked structure without disrupting(打扰) readers. 

其实，上面这段话，引起了我思考这样的问题: length of pointer and length of Word？

从上面这段话看出，length of pointer `==` length of Word，那实际的情况是这样的吗？

### stackoverflow [Is pointer assignment atomic in C++?](https://stackoverflow.com/questions/8919818/is-pointer-assignment-atomic-in-c)

[A](https://stackoverflow.com/a/8920183)

C++03 does not know about the existance of threads, therefore the concept of atomicity doesn't make much sense for C++03, meaning that it doesn't say anything about that.

C++11 does know about threads, but once again doesn't say anything about the atomicity of assigning pointers. However C++11 does contain `std::atomic<T*>`, which is guaranteed to be atomic.

Note that even if writing to a raw pointer is atomic on your platform the compiler is still free to move that assingment around, so that doesn't really buy you anything.

If you need to write to a pointer which is shared between threads use either `std::atomic<T*>` (or the not yet official `boost::atomic<T*>`, gccs atomic intrinsics or windows Interlocked*) or wrap all accesses to that pointer in mutexes.

## Assignment(=): int

在 preshing [Atomic vs. Non-Atomic Operations]( https://preshing.com/20130618/atomic-vs-non-atomic-operations/) 中也进行了讨论。

### TODO stackoverflow [Why is integer assignment on a naturally aligned variable atomic on x86?](https://stackoverflow.com/questions/36624881/why-is-integer-assignment-on-a-naturally-aligned-variable-atomic-on-x86)

### stackoverflow [Are C++ Reads and Writes of an int Atomic?](https://stackoverflow.com/questions/54188/are-c-reads-and-writes-of-an-int-atomic)

[A](https://stackoverflow.com/a/54242)

Boy, what a question. The answer to which is:

> Yes, no, hmmm, well, it depends

It all comes down to the architecture of the system. On an IA32 a correctly aligned address will be an atomic operation. Unaligned writes might be atomic, it depends on the caching system in use. If the memory lies within a single L1 cache line then it is atomic, otherwise it's not. The width of the bus between the CPU and RAM can affect the atomic nature: a correctly aligned 16bit write on an 8086 was atomic whereas the same write on an 8088 wasn't because the 8088 only had an 8 bit bus whereas the 8086 had a 16 bit bus.

Also, if you're using C/C++ don't forget to mark the shared value as volatile, otherwise the optimiser will think the variable is never updated in one of your threads.

### [Why is integer assignment on a naturally aligned variable atomic on x86?](https://stackoverflow.com/questions/36624881/why-is-integer-assignment-on-a-naturally-aligned-variable-atomic-on-x86)

**"Natural" alignment means aligned to it's own type width**. Thus, the load/store will never be split across any kind of boundary wider than itself (e.g. page, cache-line, or an even narrower chunk size used for data transfers between different caches).

CPUs often do things like cache-access, or cache-line transfers between cores, in power-of-2 sized chunks, so alignment boundaries smaller than a cache line do matter. (See @BeeOnRope's comments below). See also [Atomicity on x86](https://stackoverflow.com/questions/38447226/atomicity-on-x86) for more details on how CPUs implement atomic loads or stores internally, and [Can num++ be atomic for 'int num'?](https://stackoverflow.com/questions/39393850/can-num-be-atomic-for-int-num) for more about how atomic RMW operations like `atomic<int>::fetch_add()` / `lock xadd` are implemented internally.

## Assignment(=): bool

参见 `./Byte-granular-memory-access` 章节。
