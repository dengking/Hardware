# Compare and swap

一、在下面文章中，也涉及了这个topic:

1、preshing [An Introduction to Lock-Free Programming](https://preshing.com/20120612/an-introduction-to-lock-free-programming/)

二、在阅读了 

1、wikipedia [Optimistic concurrency control](https://en.wikipedia.org/wiki/Optimistic_concurrency_control) 

其中的一个核心观点就是: transaction 是 optimistic concurrency control

2、zhuanlan [【BAT面试题系列】面试官：你了解乐观锁和悲观锁吗？后，我觉得使用](https://zhuanlan.zhihu.com/p/74372722) 

使用CAS、MVCC来实现optimistic concurrency control，这两种方式本质上是有些类似的。

我对compare-and-swap的认知是: 

使用transaction来理解compare-and-swap: 

通过"compare"来判断在这段时间内是否发生了状态改变，如果状态改变了，则终止transaction，否则commit修改。

## wikipedia [Compare-and-swap](https://en.wikipedia.org/wiki/Compare-and-swap)

In [computer science](https://en.wikipedia.org/wiki/Computer_science), **compare-and-swap** (**CAS**) is an [atomic](https://en.wikipedia.org/wiki/Atomic_(computer_science)) [instruction](https://en.wikipedia.org/wiki/Instruction_(computer_science)) used in [multithreading](https://en.wikipedia.org/wiki/Thread_(computer_science)#Multithreading) to achieve [synchronization](https://en.wikipedia.org/wiki/Synchronization_(computer_science)). It compares the contents of a memory location with a given value and, only if they are the same, modifies the contents of that memory location to a new given value. This is done as a single **atomic operation**. The atomicity guarantees that the new value is calculated based on up-to-date information; if the value had been updated by another thread in the meantime, the write would fail. The result of the operation must indicate whether it performed the substitution; this can be done either with a simple [boolean](https://en.wikipedia.org/wiki/Boolean_logic) response (this variant is often called **compare-and-set**), or by returning the value read from the memory location (*not* the value written to it).

> NOTE: 
>
> C++ `std::compare_exchange`所做的就是 **compare-and-set**

### Overview

A compare-and-swap operation is an atomic version of the following [pseudocode](https://en.wikipedia.org/wiki/Pseudocode), where * denotes [access through a pointer](https://en.wikipedia.org/wiki/Indirection):[[1\]](https://en.wikipedia.org/wiki/Compare-and-swap#cite_note-plan9-1)

```pseudocode
function cas(p : pointer to int, old : int, new : int) returns bool {
    if *p ≠ old {
        return false
    }
    *p ← new
    return true
}
```

This operation is used to implement [synchronization primitives](https://en.wikipedia.org/wiki/Synchronization_(computer_science)#Thread_or_process_synchronization) like [semaphores](https://en.wikipedia.org/wiki/Semaphore_(programming)) and [mutexes](https://en.wikipedia.org/wiki/Mutex),[[1\]](https://en.wikipedia.org/wiki/Compare-and-swap#cite_note-plan9-1) as well as more sophisticated [lock-free and wait-free algorithms](https://en.wikipedia.org/wiki/Lock-free_and_wait-free_algorithms). [Maurice Herlihy](https://en.wikipedia.org/wiki/Maurice_Herlihy) (1991) proved that CAS can implement more of these algorithms than [atomic](https://en.wikipedia.org/wiki/Atomic_operation) read, write, or [fetch-and-add](https://en.wikipedia.org/wiki/Fetch-and-add), and assuming a fairly large[*clarification needed*] amount of memory, that it can implement all of them.[[2\]](https://en.wikipedia.org/wiki/Compare-and-swap#cite_note-herlihy91-2) CAS is equivalent to [load-link/store-conditional](https://en.wikipedia.org/wiki/Load-link/store-conditional), in the sense that a constant number of invocations of either primitive can be used to implement the other one in a [wait-free](https://en.wikipedia.org/wiki/Wait-free) manner.[[3\]](https://en.wikipedia.org/wiki/Compare-and-swap#cite_note-3)

> NOTE : 翻译如下:
>
> "此操作用于实现信号量和互斥量等同步原语，以及更复杂的无锁和无等待算法。 Maurice Herlihy（1991）证明了CAS可以实现更多这些算法而不是原子读取，写入或获取和添加，并且假设存储量相当大，它可以实现所有这些算法.CAS等同于加载 -  link / store-conditional，在某种意义上，可以使用任一原语的常量调用来以无等待的方式实现另一个原语。"

Algorithms built around CAS typically read some key memory location and remember the old value. Based on that old value, they compute some new value. Then they try to swap in the new value using CAS, where the comparison checks for the location still being equal to the old value. If CAS indicates that the attempt has failed, it has to be repeated from the beginning: the location is re-read, a new value is re-computed and the CAS is tried again. Instead of immediately retrying after a CAS operation fails, researchers have found that total system performance can be improved in multiprocessor systems—where many threads constantly update some particular shared variable—if threads that see their CAS fail use [exponential backoff](https://en.wikipedia.org/wiki/Exponential_backoff)—in other words, wait a little before retrying the CAS.[[4\]](https://en.wikipedia.org/wiki/Compare-and-swap#cite_note-dice-4)

### Example application: atomic adder



```C++
function add(p : pointer to int, a : int) returns int
{
	done ← false
	while not done
	{
		value ← *p  // Even this operation doesn't need to be atomic.
		done ← cas(p, value, value + a)
	}
	return value + a
}

```



> NOTE: 
>
> 1、如果 p 和 value相等(说明这段时间内没有其他的transaction发生)，则将value + p 递交；否则不递交，终止。

In this algorithm, if the value of `*p` changes after (or while!) it is fetched and before the CAS does the store, CAS will notice and report this fact, causing the algorithm to retry.[[5\]](https://en.wanweibaike.com/wiki-Compare-And-Swap#cite_note-5)



## 劣势 和 局限性

### 1、zhihu [【BAT面试题系列】面试官：你了解乐观锁和悲观锁吗？](https://zhuanlan.zhihu.com/p/74372722)

下面是CAS一些不那么完美的地方：

#### 1、ABA问题

在`AtomicInteger`的例子中，ABA似乎没有什么危害。但是在某些场景下，ABA却会带来隐患，例如栈顶问题：一个栈的栈顶经过两次(或多次)变化又恢复了原值，但是栈可能已发生了变化。

对于ABA问题，比较有效的方案是引入版本号，内存中的值每发生一次变化，版本号都+1；在进行CAS操作时，不仅比较内存中的值，也会比较版本号，只有当二者都没有变化时，CAS才能执行成功。Java中的`AtomicStampedReference`类便是使用版本号来解决ABA问题的。

#### 2、高竞争下的开销问题

在并发冲突概率大的高竞争环境下，如果CAS一直失败，会一直重试，CPU开销较大。针对这个问题的一个思路是引入退出机制，如重试次数超过一定阈值后失败退出。当然，更重要的是避免在高竞争环境下使用乐观锁。

#### 3、功能限制

CAS的功能是比较受限的，例如CAS只能保证单个变量（或者说单个内存值）操作的原子性，这意味着：(1)原子性不一定能保证线程安全，例如在Java中需要与volatile配合来保证线程安全；(2)当涉及到多个变量(内存值)时，CAS也无能为力。

> NOTE: 
>
> 1、多个CAS  optimistic lock无法工作

除此之外，CAS的实现需要硬件层面处理器的支持，在Java中普通用户无法直接使用，只能借助atomic包下的原子类使用，灵活性受到限制。
