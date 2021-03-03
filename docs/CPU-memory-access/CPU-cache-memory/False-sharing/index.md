# False sharing

是在阅读 zhuanlan.zhihu [高并发编程--多处理器编程中的一致性问题(上)](https://zhuanlan.zhihu.com/p/48157076 ) 时，其中提及了false sharing，并进行了非常好的描述。

## zhuanlan.zhihu [高并发编程--多处理器编程中的一致性问题(上)](https://zhuanlan.zhihu.com/p/48157076 ) 

### cacheline导致的false sharing

```cpp
struct test {
  uint32_t a;
  uint32_t b;
 }
 test t;
```

`t`是一个共享变量，core1上线程t1访问`t.a`， core2线程t2访问`t.b`,如果cacheline大小是16Bytes，那么当t1访问`t.a`时cache miss，然后会将这个`t.a`读取到cache中，但是为了提高效率，core每次读取的长度是cacheline的长度，`t.a`长度是4， `t.b`长度是4，所以在一次读取中会将`t.a`和`t.b`一次性读到了自己cache中了，那么t2也同样存在这个问题，如果t1修改当前cacheline中的值，他需要与core2对应的cacheline同步，那么本来两个可以并发的memory location引入了不必要的同步。对性能产生不必要的影响。

那么为了避免**false sharing**，就需要让这两个memory location进行cacheline对齐。C++ 提供了**alignas**的方法。

## dzone [False Sharing](https://dzone.com/articles/false-sharing)

Memory is stored within the cache system in units know as **cache lines**. **Cache lines** are a power of 2 of contiguous bytes which are typically 32-256 in size. The most common **cache line size** is 64 bytes.  

**False sharing** is a term which applies when threads unwittingly(不知不觉地) impact the performance of each other while modifying independent variables sharing the same **cache line**. Write contention on cache lines is the single most limiting factor on achieving scalability for parallel threads of execution in an SMP system. I’ve heard **false sharing** described as the silent performance killer because it is far from obvious when looking at code.

> NOTE: 这段话非常具有启示作用

### To achieve linear scalability with number of threads

> NOTE: 这段总结非常好
>
> 1、如何保证"no two threads write to the same variable or **cache line**"?
>
> 保证"no two threads write to the same variable"是相对容易的，但是后者的实现则相对麻烦一些。其实这就是如何detect false sharing。

To achieve linear scalability with number of threads, we must ensure no two threads write to the same variable or **cache line**. Two threads writing to the same variable can be tracked down at a code level.  To be able to know if independent variables share the same **cache line** we need to know the **memory layout**, or we can get a tool to tell us. **Intel VTune** is such a profiling tool. In this article I’ll explain how memory is laid out for Java objects and how we can pad(补齐) out our cache lines to avoid false sharing.

> NOTE: 最后一段话的翻译如下:
>
> "在本文中，我将解释如何为Java对象布局内存，以及如何扩展缓存线以避免错误共享。"

[![img](http://1.bp.blogspot.com/-MYso5dR5ItE/TjOy1Mbr3NI/AAAAAAAAAAk/uLPjSeGhbmg/s1600/cache-line.png)](http://1.bp.blogspot.com/-MYso5dR5ItE/TjOy1Mbr3NI/AAAAAAAAAAk/uLPjSeGhbmg/s1600/cache-line.png)

Figure 1. above illustrates the issue of **false sharing**. A thread running on core 1 wants to update variable `X` while a thread on core 2 wants to update variable `Y`. Unfortunately these two **hot variables** reside in the same **cache line**. Each thread will race for ownership of the **cache line** so they can update it. If core 1 gets ownership then the cache sub-system will need to invalidate the corresponding cache line for core 2. When Core 2 gets ownership and performs its update, then core 1 will be told to invalidate its copy of the cache line. This will ping pong back and forth via the **L3 cache** greatly impacting performance. The issue would be further exacerbated(加重) if competing cores are on different sockets and additionally have to cross the socket interconnect.

### Java Memory Layout

For the Hotspot JVM, all objects have a 2-word header. 

First is the “mark” word which is made up of 24-bits for the **hash code** and 8-bits for flags such as the **lock state**, or it can be swapped for lock objects. 

The second is a reference to the class of the object. Arrays have an additional word for the size of the array. 

Every object is aligned to an 8-byte granularity boundary for performance. Therefore to be efficient when packing, the object fields are re-ordered from declaration order to the following order based on size in bytes:

1、doubles (8) and longs (8)

2、ints (4) and floats (4)

3、shorts (2) and chars (2)

4、booleans (1) and bytes (1)

5、references (4/8)

With this knowledge we can pad a cache line between any fields with 7 longs. Within the [Disruptor](http://code.google.com/p/disruptor/) we pad cache lines around the [RingBuffer ](http://code.google.com/p/disruptor/source/browse/trunk/code/src/main/com/lmax/disruptor/RingBuffer.java)cursor and [BatchEventProcessor](http://code.google.com/p/disruptor/source/browse/trunk/code/src/main/com/lmax/disruptor/BatchEventProcessor.java) sequences.

To show the performance impact let’s take a few threads each updating their own independent counters. These counters will be *volatile long*s so the world can see their progress.



```C++
public final class FalseSharing
    implements Runnable
{
    public final static int NUM_THREADS = 4; // change
    public final static long ITERATIONS = 500L * 1000L * 1000L;
    private final int arrayIndex;

    private static VolatileLong[] longs = new VolatileLong[NUM_THREADS];
    static
    {
        for (int i = 0; i < longs.length; i++)
        {
            longs[i] = new VolatileLong();
        }
    }

    public FalseSharing(final int arrayIndex)
    {
        this.arrayIndex = arrayIndex;
    }

    public static void main(final String[] args) throws Exception
    {
        final long start = System.nanoTime();
        runTest();
        System.out.println("duration = " + (System.nanoTime() - start));
    }

    private static void runTest() throws InterruptedException
    {
        Thread[] threads = new Thread[NUM_THREADS];

        for (int i = 0; i < threads.length; i++)
        {
            threads[i] = new Thread(new FalseSharing(i));
        }

        for (Thread t : threads)
        {
            t.start();
        }

        for (Thread t : threads)
        {
            t.join();
        }
    }

    public void run()
    {
        long i = ITERATIONS + 1;
        while (0 != --i)
        {
            longs[arrayIndex].value = i;
        }
    }

    public final static class VolatileLong
    {
        public volatile long value = 0L;
        public long p1, p2, p3, p4, p5, p6; // comment out
    }
}
```



**Results**

Running the above code while ramping(提升) the number of threads and adding/removing the **cache line padding**, I get the results depicted in Figure 2. below. This is measuring the duration of test runs on my 4-core Nehalem at home.



| [![img](http://2.bp.blogspot.com/-gi_3N6LC26E/TjOzSGIUMxI/AAAAAAAAAAo/5-UaNKmGZzY/s1600/duration.png)](http://2.bp.blogspot.com/-gi_3N6LC26E/TjOzSGIUMxI/AAAAAAAAAAo/5-UaNKmGZzY/s1600/duration.png) |
| ------------------------------------------------------------ |
| Figure 2.                                                    |


The impact of false sharing can clearly be seen by the increased execution time required to complete the test. Without the **cache line contention** we achieve near linear scale up with threads.

This is not a perfect test because we cannot be sure where the *VolatileLongs* will be laid out in memory. They are independent objects. However experience shows that objects allocated at the same time tend to be co-located.

So there you have it. False sharing can be a silent performance killer.



## ustc.edu [False Sharing](https://scc.ustc.edu.cn/zlsc/sugon/intel/compiler_c/main_cls/cref_cls/common/cilk_false_sharing.htm)

False sharing is a common problem in **shared memory parallel processing**. It occurs when two or more cores hold a copy of the same **memory cache line**.

> NOTE: 
>
> 1、总结得比较好

If one core writes, the **cache line** holding the **memory line** is invalidated on other cores. Even though another core may not be using that data (reading or writing), it may be using another element of data on the same cache line. The second core will need to reload the line before it can access its own data again.

### Detect false sharing

The cache hardware ensures **data coherency**, but at a potentially high performance cost if **false sharing** is frequent. A good technique to identify false sharing problems is to catch unexpected sharp increases in last-level **cache misses** using hardware counters or other performance tools.

> NOTE: 
>
> 1、这段话让我想到了cache Miss 和 false sharing的关联

### Simple example

As a simple example, consider a spawned function with a for loop that increments array values. The array is volatile to force the compiler to generate store instructions rather than hold values in registers or optimize the loop.

> NOTE: 
>
> 1、此处对`volatile` 的解释、使用是非常好的

```C++
volatile int x[32];

void f(volatile int *p)
{
   for (int i = 0; i < 100000000; i++)
   {
     ++p[0];
     ++p[16];
   }
}

int main()
{
   cilk_spawn f(&x[0]);
   cilk_spawn f(&x[1]);
   cilk_spawn f(&x[2]);
   cilk_spawn f(&x[3]);
   cilk_sync;
   return 0;
 }
```

> NOTE: 
>
> 1、上述例子使用了`Cilk`语言，参见 :
>
> a、https://cilk.mit.edu/programming/
>
> b、wikipedia [Cilk](https://en.wikipedia.org/wiki/Cilk)
>
> 2、
>
> `cilk_spawn` 其实就相当于 创建了一个thread，它对应的的`pthread_create`
>
> `cilk_sync` 对应的是`pthrread_join`
>
> 显然上述例子是典型的: [fork–join idiom](https://en.wikipedia.org/wiki/Fork–join_model)
>
> 3、上述例子非常好地展示了false sharing:
>
> 通过下面的描述"64-byte cache line"可知，cache line是64-byte，因此对应的是16个int，而`void f(volatile int *p)`的实现为: 
>
> ```c++
>      ++p[0];
>      ++p[16];
> ```
>
> 显然这是17个int，显然它就会触发"cache line contention"。

The `x[]` elements are four bytes wide, and a 64-byte cache line would hold 16 elements. There are no data races, and the results will be correct when the loop completes. However, cache line contention as the individual strands update adjacent array elements can degrade performance, sometimes significantly.



## wikipedia [False sharing](https://en.wikipedia.org/wiki/False_sharing)

In [computer science](https://en.wikipedia.org/wiki/Computer_science), **false sharing** is a performance-degrading usage pattern that can arise in systems with distributed, [coherent caches](https://en.wikipedia.org/wiki/Cache_coherence) at the size of the smallest resource block managed by the caching mechanism. When a system participant attempts to periodically access data that will never be altered by another party, but those data share a cache block with data that *are* altered, the caching protocol may force the first participant to reload the whole unit despite a lack of logical necessity. The caching system is unaware of activity within this block and forces the first participant to bear the caching system overhead required by true shared access of a resource.

> NOTE: 
>
> 1、相比于前面两篇文章的具体，wikipedia [False sharing](https://en.wikipedia.org/wiki/False_sharing) 的上述内容就非常抽象了:
>
> a、"the size of the smallest resource block managed by the caching mechanism" 对应的是 size of cache line，其实就是unit



## TODO

*Intel*. [Avoiding and Identifying False Sharing Among Threads](https://software.intel.com/en-us/articles/avoiding-and-identifying-false-sharing-among-threads).