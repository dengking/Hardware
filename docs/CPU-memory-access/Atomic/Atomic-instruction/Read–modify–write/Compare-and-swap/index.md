# Compare and swap

典型的

## wikipedia [Compare-and-swap](https://en.wikipedia.org/wiki/Compare-and-swap)

In [computer science](https://en.wikipedia.org/wiki/Computer_science), **compare-and-swap** (**CAS**) is an [atomic](https://en.wikipedia.org/wiki/Atomic_(computer_science)) [instruction](https://en.wikipedia.org/wiki/Instruction_(computer_science)) used in [multithreading](https://en.wikipedia.org/wiki/Thread_(computer_science)#Multithreading) to achieve [synchronization](https://en.wikipedia.org/wiki/Synchronization_(computer_science)). It compares the contents of a memory location with a given value and, only if they are the same, modifies the contents of that memory location to a new given value. This is done as a single **atomic operation**. The atomicity guarantees that the new value is calculated based on up-to-date information; if the value had been updated by another thread in the meantime, the write would fail. The result of the operation must indicate whether it performed the substitution; this can be done either with a simple [boolean](https://en.wikipedia.org/wiki/Boolean_logic) response (this variant is often called **compare-and-set**), or by returning the value read from the memory location (*not* the value written to it).



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

> NOTE : 此操作用于实现信号量和互斥量等同步原语，以及更复杂的无锁和无等待算法。 Maurice Herlihy（1991）证明了CAS可以实现更多这些算法而不是原子读取，写入或获取和添加，并且假设存储量相当大，它可以实现所有这些算法.CAS等同于加载 -  link / store-conditional，在某种意义上，可以使用任一原语的常量调用来以无等待的方式实现另一个原语。

Algorithms built around CAS typically read some key memory location and remember the old value. Based on that old value, they compute some new value. Then they try to swap in the new value using CAS, where the comparison checks for the location still being equal to the old value. If CAS indicates that the attempt has failed, it has to be repeated from the beginning: the location is re-read, a new value is re-computed and the CAS is tried again. Instead of immediately retrying after a CAS operation fails, researchers have found that total system performance can be improved in multiprocessor systems—where many threads constantly update some particular shared variable—if threads that see their CAS fail use [exponential backoff](https://en.wikipedia.org/wiki/Exponential_backoff)—in other words, wait a little before retrying the CAS.[[4\]](https://en.wikipedia.org/wiki/Compare-and-swap#cite_note-dice-4)

