# Byte-granular-memory-access

本文讨论字节粒度的memory access。

## Byte-granular update atomic?

访问单字节是否是atomic的？

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





### stackoverflow [Can modern x86 hardware not store a single byte to memory?](https://stackoverflow.com/questions/46721075/can-modern-x86-hardware-not-store-a-single-byte-to-memory)

[A](https://stackoverflow.com/a/46818162)

> NOTE: 这个回答非常详细

**TL:DR: On every modern ISA that has byte-store instructions (including x86), they're atomic and don't disturb surrounding bytes.** (I'm not aware of any older ISAs where byte-store instructions could "invent writes" to neighbouring bytes either.)

> NOTE: 上述是结论

[A](https://stackoverflow.com/a/46722180)

Not only are x86 CPUs capable of reading and writing a single byte, all modern general purpose CPUs are capable of it. More importantly most modern CPUs (including x86, ARM, MIPS, PowerPC, and SPARC) are capable of atomically reading and writing single bytes.