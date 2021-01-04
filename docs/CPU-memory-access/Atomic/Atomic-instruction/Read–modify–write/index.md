# Read-modify-write



## Wikipedia [Read-modify-write](https://infogalactic.com/info/Read-modify-write)

In [computer science](https://infogalactic.com/info/Computer_science), **read-modify-write** is a class of [atomic operations](https://infogalactic.com/info/Atomic_operation) (such as [test-and-set](https://infogalactic.com/info/Test-and-set), [fetch-and-add](https://infogalactic.com/info/Fetch-and-add), and [compare-and-swap](https://infogalactic.com/info/Compare-and-swap)) that both read a memory location and write a new value into it simultaneously(同时), either with a completely new value or some function of the previous value. 

> NOTE: "simultaneously"其实就意味着 "原子性" 。"some function of previous value" 的意思是 将previous value作为入参输入到一个function中而得到一个新value，然后将这个新value写入到memory中

These operations prevent [race conditions](https://infogalactic.com/info/Race_conditions) in multi-threaded applications. Typically they are used to implement [mutexes](https://infogalactic.com/info/Mutex) or [semaphores](https://infogalactic.com/info/Semaphore_(programming)). These atomic operations are also heavily used in [non-blocking synchronization](https://infogalactic.com/info/Non-blocking_synchronization).

> NOTE: 我们平时所使用的mutex、semaphore都是基于这些atomic operation而实现的。

[Maurice Herlihy](https://infogalactic.com/info/Maurice_Herlihy) (1991) ranks atomic operations by their *[consensus](https://infogalactic.com/info/Consensus_(computer_science)) numbers,* as follows:

> NOTE: 在CPU中，也可以使用multiple model来进行描述，因此它也可以使用consensus得到概念

| rank      |                                                              |
| --------- | ------------------------------------------------------------ |
| *∞*       | memory-to-memory move and swap, augmented queue, [compare-and-swap](https://infogalactic.com/info/Compare-and-swap), [fetch-and-cons](https://infogalactic.com/w/index.php?title=Fetch-and-cons&action=edit&redlink=1), [sticky byte](https://infogalactic.com/w/index.php?title=Sticky_byte&action=edit&redlink=1), [load-link/store-conditional](https://infogalactic.com/info/Load-link/store-conditional) (LL/SC)[[1\]](https://infogalactic.com/info/Read-modify-write#cite_note-1) |
| *2n - 2*: | n-register assignment                                        |
| *2*       | [test-and-set](https://infogalactic.com/info/Test-and-set), swap, [fetch-and-add](https://infogalactic.com/info/Fetch-and-add), queue, stack |
| *1*       | atomic read and atomic write                                 |



> NOTE:上述自底向上依次增长

It is impossible to implement an operation that requires a given **consensus number** with only operations with a lower **consensus number**, no matter how many of such operations one uses.[[2\]](https://infogalactic.com/info/Read-modify-write#cite_note-2)

**Read-modify-write instructions** often produce unexpected results when used on [I/O](https://infogalactic.com/info/I/O) devices, as a write operation may not affect the same internal [register](https://infogalactic.com/info/Hardware_register) that would be accessed in a read operation.[[3\]](https://infogalactic.com/info/Read-modify-write#cite_note-3)



## stackoverflow [Why it's termed read-modify-write but not read-write?](https://stackoverflow.com/questions/49452022/why-its-termed-read-modify-write-but-not-read-write)



### [A](https://stackoverflow.com/a/49638936)

Because that is exactly the sequence of events on a typical architecture such as `X86`.

1. *read*: The value is read from a memory location (cache) into a CPU register
2. *modify*: The value is incremented inside the CPU register
3. *write*: The updated register value is written back to memory (cache).

In order to create the perception of atomicity, the cache line is locked between the `read` and the `write` operation.

For example, incrementing a C++ atomic variable:

```C++
g.fetch_add(1);
```

Is compiled by `gcc` into:

```assembly
0x00000000004006c0 <+0>:     lock addl $0x1,0x200978(%rip)        # 0x601040 <g>
```

Despite being a single instruction, `addl` is not atomic by itself. The `lock` prefix is necessary to guarantee that the updated register value is written back to the cache line before it can be accessed by other cores (the store buffer is flushed, but bypassed for RMW operations).

The **`MESI` cache coherence protocol** ensures that all cores observe the updated memory value after the lock on the cache line has been released. This guarantees that all threads observe the latest value in the modification order which is required for RMW operations by the C++ standard.

> NOTE: 解释得非常好。



## Compare-and-swap VS test-and-set

stackoverflow [compare and swap vs test and set](https://stackoverflow.com/questions/3659336/compare-and-swap-vs-test-and-set)

[A](https://stackoverflow.com/a/3659535): 

> `test-and-set` modifies the contents of a memory location and returns its old value as a single atomic operation.
>
> `compare-and-swap` atomically compares the contents of a memory location to a given value and, **only if they are the same**, modifies the contents of that memory location to a given new value.
>
> *The difference marked in bold.*

[A](https://stackoverflow.com/a/40101529):

> **Test and set operates on a bit, compare and swap operates on a 32-bit field.**