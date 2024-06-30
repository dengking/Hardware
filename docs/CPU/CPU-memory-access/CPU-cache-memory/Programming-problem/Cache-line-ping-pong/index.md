# Cache line ping-pong

总的来说，原因如下:

1、true sharing: 对于"[CPU](https://everything2.com/title/CPU)s that have local [cache](https://everything2.com/title/cache)s"，multiple CPUs are working on the same set of data from main memory

2、false sharing

## everything2 [cache line ping-pong](https://everything2.com/title/cache+line+ping-pong)

One way of maintaining [cache coherence](https://everything2.com/title/cache+coherence) in [multiprocessing](https://everything2.com/title/multiprocessing) designs with [CPU](https://everything2.com/title/CPU)s that have local [cache](https://everything2.com/title/cache)s is to ensure that single [cache line](https://everything2.com/title/cache+line)s are never held by more than one CPU at a time. With [write-through cache](https://everything2.com/title/write-through+cache)s, this is easily implemented by having the CPUs invalidate cache lines on [snoop](https://everything2.com/title/snooping) hits.

> NOTE: 
>
> 一、这一点在 cnblogs [Cache ping-pong](https://www.cnblogs.com/fengshang/p/3524752.html) 中也有描述
>
> 二、关于 [snoop](https://everything2.com/title/snooping) ，还可以参见 wikipedia [Bus snooping](https://en.wikipedia.org/wiki/Bus_snooping)
>
> 三、这篇文章的介绍是非常好的

However, if multiple CPUs are working on the same set of data from main memory, this can lead to the following scenario:

1、CPU `#1` reads a cache line from memory.

2、CPU `#2` reads the same line, CPU #1 snoops the access and invalidates its local copy.

3、CPU `#1` needs the data again and has to re-read the *entire* cache line, invalidating the copy in CPU #2 in the process.

4、CPU `#2` now also re-reads the entire line, invalidating the copy in CPU #1.

5、[Lather, rinse, repeat](https://everything2.com/title/Lather%2C+rinse%2C+repeat).

> NOTE: 
>
> 泡沫、冲洗、重复

The result is a dramatic [performance](https://everything2.com/title/performance) loss because the CPUs keep fetching the same data over and over again from slow main memory.

Possible solutions include:

1、Use a smarter cache coherence protocol, such as [MESI](https://everything2.com/title/MESI+protocol).

2、Mark the address space in question as cache-inhibited(禁止的). Most CPUs will then resort to single-word accesses which should be faster than reloading entire cache lines (usually 32 or 64 bytes).

3、If the data set is small, make one copy in memory for each CPU.

4、If the data set is large and processed sequentially, have each CPU work on a different part of it (one starting at the beginning, one at the middle, etc.).

## cnblogs [Cache ping-pong](https://www.cnblogs.com/fengshang/p/3524752.html)

![img](https://images0.cnblogs.com/blog/599660/201401/180059327980.png)

这样做读取速度是快了，但是在并发编程时就会遇到问题，如果任何一个核对数据有写操作，如何保证所有core都能及时看到新的数据？考虑如下程序：

```C++
std::atomic<int> counter{0};

void thread_func(void)
{
     counter++;
}
```

> NOTE: 
>
> 原文这里的描述其实和 everything2 [cache line ping-pong](https://everything2.com/title/cache+line+ping-pong) 中的介绍基本类似，可以pass

### Cache ping-pong

当多个core要并行操作内存中的同一份数据，就会出现Cache ping-pong的问题，举个例子：

\1. core1 从内存中加载Cache line做操作

\2. core2 也要对这个数据操作，于是它也加载了这个Cache line。core1发现core2加载了这个cache line，于是让自己的加载的cache line失效

\3. core1 又需要处理这个数据，core1需要重新从内存中加载这个cache line，同时导致core2的cache line失效

\4. core2 又要处理这个数据，又要加载cache line，导致core1的cache line失效

\5. 如此反复。

cache ping-pong等效于cache失效，每次CPU都要从RAM中加载数据，这会导致很大的性能问题，这也是编写多线程程序需要考虑的设计问题。应该尽量避免代码中多个线程对同一数据资源的竞争。

### False sharing

即使两个线程不操作同一个数据，也可能会导致cache ping-pong的问题，因为cache line的存在。考虑以下结构体：

```c++
struct flags {
      bool pushed;
      bool popped;
};
```

由于pushed和popped内存分布极为接近，它们几乎总会在一个cache line中被加载（cache line可能为32-128字节），如果两个core并行操作，一个修改pushed，一个修改popped，虽然修改的变量不同，一样会导致cache line ping-pong，因为cache line是一个整体（当然支持cache line能修改单字节而不是整体加载的CPU体系结构可能没有这个问题). 这就是False sharing的问题：虽然并行访问不同对象，但却像是访问同一个对象一样引起cache line ping-pong。

简单的解决方法通常也很粗暴——通过添加padding，强制pushed和popped分布在不同的cache line上，例如下面这样定义flags：

```c++
struct flags {
      bool pushed[N];
      char pad[LINE_SIZE];
      bool popped[N];
} states;
```

 