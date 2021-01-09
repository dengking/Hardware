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



### stackoverflow [is assignment operator '=' atomic?](https://stackoverflow.com/questions/8290768/is-assignment-operator-atomic)

I'm implementing Inter-Thread Communication using global variable.

```cpp
//global var
volatile bool is_true = true;

//thread 1
void thread_1()
{
    while(1){
        int rint = rand() % 10;
        if(is_true) {
            cout << "thread_1: "<< rint <<endl;  //thread_1 prints some stuff
            if(rint == 3)
                is_true = false;  //here, tells thread_2 to start printing stuff
        }
    }
}

//thread 2
void thread_2()
{
    while(1){
        int rint = rand() % 10;
        if(! is_true) {  //if is_true == false
            cout << "thread_1: "<< rint <<endl;  //thread_2 prints some stuff
            if(rint == 7)  //7
                is_true = true;  //here, tells thread_1 to start printing stuff
        }
    }
}

int main()
{
    HANDLE t1 = CreateThread(0,0, thread_1, 0,0,0);
    HANDLE t2 = CreateThread(0,0, thread_2, 0,0,0);
    Sleep(9999999);
    return 0;
}
```

**Question**

In the code above, I use a global var `volatile bool is_true` to switch printing between thread_1 and thread_2.

I wonder **whether it is thread-safe to use assignment operation here**?

[A](https://stackoverflow.com/a/8291193)

This code is not guaranteed to be thread-safe on Win32, since Win32 guarantees **atomicity** only for **properly-aligned 4-byte** and **pointer-sized** values. `bool` is not guaranteed to be one of those types. (It is typically a 1-byte type.)

> NOTE: memory access `bool`是能够保证atomicity，但是并不能保证thread safe，在前面的"Atomic operation and thread safe"中对这个话题进行了讨论

For those who demand an actual example of how this could fail:

Suppose that `bool` is a 1-byte type. Suppose also that your `is_true` variable happens to be stored adjacent to another `bool` variable (let's call it `other_bool`), so that both of them share the same 4-byte line. For concreteness, let's say that `is_true` is at address `0x1000` and `other_bool` is at address `0x1001`. Suppose that both values are initially `false`, and one thread decides to update `is_true` at the same time another thread tries to update `other_bool`. The following sequence of operations can occur:

> NOTE: `is_true`、`other_bool`是adjacent的，因此CPU一次read，能够将两者同时读到register中。

1、Thread 1 prepares to set `is_true` to `true` by loading the 4-byte value containing `is_true` and `other_bool`. Thread 1 reads `0x00000000`.

2、Thread 2 prepares to set `other_bool` to `true` by loading the 4-byte value containing `is_true` and `other_bool`. Thread 2 reads `0x00000000`.

3、Thread 1 updates the byte in the 4-byte value corresponding to `is_true`, producing `0x00000001`.

4、Thread 2 updates the byte in the 4-byte value corresponding to `other_bool`, producing `0x00000100`.

5、Thread 1 stores the updated value to memory. `is_true` is now `true` and `other_bool` is now `false`.

6、Thread 2 stores the updated value to memory. `is_true` is now `false` and `other_bool` is now `true`.

Observe that at the end this sequence, the update to `is_true` was lost, because it was overwritten by thread 2, which captured an old value of `is_true`.

It so happens that x86 is very forgiving of this type of error because it supports byte-granular updates and has a very tight memory model. Other Win32 processors are not as forgiving. RISC chips, for example, often do not support byte-granular updates, and even if they do, they usually have very weak memory models.

### stroustrup C++11FAQ [Memory model](https://www.stroustrup.com/C++11FAQ.html#memory-model)

> NOTE: 其实列举了和 stackoverflow [is assignment operator '=' atomic?](https://stackoverflow.com/questions/8290768/is-assignment-operator-atomic) 中类似的例子。在其中给出了C++中的规定

So, C++11 guarantees that no such problems occur for "**separate memory locations**". More precisely: **A memory location cannot be safely accessed by two threads without some form of locking unless they are both read accesses**. 