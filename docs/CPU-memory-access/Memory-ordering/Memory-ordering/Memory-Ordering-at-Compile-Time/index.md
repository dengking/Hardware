# Memory Ordering at Compile Time



## preshing [Memory Ordering at Compile Time](https://c.com/20120625/memory-ordering-at-compile-time/)

Between the time you type in some C/C++ source code and the time it executes on a CPU, the memory interactions of that code may be reordered according to certain rules. Changes to memory ordering are made both by the compiler (at compile time) and by the processor (at run time), all in the name of making your code run faster.

The cardinal(主要的) rule of **memory reordering**, which is universally followed by compiler developers and CPU vendors, could be phrased as follows:

> *Thou shalt not modify the behavior of a single-threaded program.*

> NOTE: 不能修改单线程程序的行为。

As a result of this rule, **memory reordering** goes largely **unnoticed** by programmers writing single-threaded code. It often goes unnoticed in multithreaded programming, too, since **mutexes**, **semaphores** and events are all designed to prevent **memory reordering** around their call sites. It’s only when **lock-free** techniques are used – when memory is shared between threads without any kind of mutual exclusion – that the cat is finally out of the bag, and the effects of memory reordering [can be plainly observed](http://preshing.com/20120515/memory-reordering-caught-in-the-act).

> NOTE: 只有当使用了无锁技术时——当内存在线程之间共享而不存在任何互斥现象时——问题才最终得到解决，并且内存重新排序的效果可以清楚地观察到。

Mind you, it is possible to write **lock-free** code for multicore platforms without the hassles(麻烦事) of **memory reordering**. As I mentioned in my [introduction to lock-free programming](http://preshing.com/20120612/an-introduction-to-lock-free-programming), one can take advantage of **sequentially consistent types**, such as `volatile` variables in Java or **C++11 atomics** – possibly at the price of a little performance. I won’t go into detail about those here. In this post, I’ll focus on the impact of the compiler on memory ordering for regular, **non-sequentially-consistent types**.



## Thoughts

### Compiler memory optimization

compiler能够全局地、高屋建瓴地分析我们的程序，能够通过对memory操作顺序来进行optimization。需要整理这方面文章。