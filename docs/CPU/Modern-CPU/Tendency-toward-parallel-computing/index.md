# 关于本章

本章讨论CPU的发展。

## 发展概述: 从[frequency scaling](https://infogalactic.com/info/Frequency_scaling) 到 [parallel scaling](https://infogalactic.com/w/index.php?title=Parallel_scaling&action=edit&redlink=1)

在Wikipedia [Parallel computing](https://infogalactic.com/info/Parallel_computing) 中对此进行了总结: 

> Parallelism has been employed for many years, mainly in [high-performance computing](https://infogalactic.com/info/High_performance_computing), but interest in it has grown lately due to the physical constraints preventing [frequency scaling](https://infogalactic.com/info/Frequency_scaling).[[2\]](https://infogalactic.com/info/Parallel_computing#cite_note-2) As power consumption (and consequently heat generation) by computers has become a concern in recent years,[[3\]](https://infogalactic.com/info/Parallel_computing#cite_note-3) parallel computing has become the dominant paradigm in [computer architecture](https://infogalactic.com/info/Computer_architecture), mainly in the form of [multi-core processors](https://infogalactic.com/info/Multi-core_processor).[[4\]](https://infogalactic.com/info/Parallel_computing#cite_note-View-Power-4)

在Wikipedia [Frequency scaling](https://infogalactic.com/info/Frequency_scaling) 中也详细的讨论了这个问题:

> Frequency ramping was the dominant force in commodity processor performance increases from the mid-1980s until roughly the end of 2004.
>
> Increasing processor [power consumption](https://infogalactic.com/info/Power_consumption) led ultimately to [Intel](https://infogalactic.com/info/Intel)'s May 2004 cancellation of its [Tejas and Jayhawk](https://infogalactic.com/info/Tejas_and_Jayhawk) processors, which is generally cited as the end of frequency scaling as the dominant computer architecture paradigm.[[3\]](https://infogalactic.com/info/Frequency_scaling#cite_note-3)
>
> > NOTE: 标志着frequency scaling的退出
>
> [Moore's Law](https://infogalactic.com/info/Moore's_Law), despite predictions of its demise, is still in effect. Despite power issues, transistor densities are still doubling every 18 to 24 months. With the end of **frequency scaling**, these new transistors (which are no longer needed to facilitate frequency scaling) can be used to add extra hardware, such as additional cores, to facilitate parallel computing - a technique that is being referred to as [parallel scaling](https://infogalactic.com/w/index.php?title=Parallel_scaling&action=edit&redlink=1).
>
> > NOTE: 可以预见: 只要[Moore's Law](https://infogalactic.com/info/Moore's_Law)依然有效，[multicore processors](https://infogalactic.com/info/Multi-core_(computing))的number of core将会进一步增加，这将进一步增加了computer的parallel computing能力。
> >
> > 最后一段，作者所总结的**parallel scaling**是非常好的。
>
> The end of frequency scaling as the dominant cause of processor performance gains has caused an industry-wide shift to [parallel computing](https://infogalactic.com/info/Parallel_computing) in the form of [multicore processors](https://infogalactic.com/info/Multi-core_(computing)).

在 gotw [The Free Lunch Is Over: A Fundamental Turn Toward Concurrency in Software](http://www.gotw.ca/publications/concurrency-ddj.htm) 中，有如下描述:

> The major processor manufacturers and architectures, from Intel and AMD to Sparc and PowerPC, have run out of room with most of their traditional approaches to boosting CPU performance. Instead of driving clock speeds and straight-line instruction throughput ever higher, they are instead turning *en masse* to hyperthreading and multicore architectures. Both of these features are already available on chips today; in particular, multicore is available on current PowerPC and Sparc IV processors, and is coming in 2005 from Intel and AMD. Indeed, the big theme of the 2004 In-Stat/MDR Fall Processor Forum was multicore devices, as many companies showed new or updated multicore processors. Looking back, it’s not much of a stretch to call 2004 the year of multicore.
>
> And that puts us at a fundamental turning point in software development, at least for the next few years and for applications targeting general-purpose desktop computers and low-end servers (which happens to account for the vast bulk of the dollar value of software sold today). In this article, I’ll describe the changing face of hardware, why it suddenly does matter to software, and how specifically the concurrency revolution matters to you and is going to change the way you will likely be writing software in the future.

在 preshing [A Look Back at Single-Threaded CPU Performance](https://preshing.com/20120208/a-look-back-at-single-threaded-cpu-performance/) 中，也进行了介绍。

显然，主要问题是无法无限制地进行frequency scaling，因此最终的结果是: "parallel computing has become the dominant paradigm in [computer architecture](https://infogalactic.com/info/Computer_architecture)"。

### 影响

当代CPU朝着parallel computing的方向的不断发展，由于它是底层的(hardware层)，因此它对computer science的其他各个领域都产生了重大的影响，它促使了computer science中的其他领域都朝着这个方向演进:

**对programming language的影响**

Programming language需要不断地引入新的特性来促进对parallel computing的充分利用，下面是一些例子:

1) C++11引入了thread、C++20引入了coroutine

> NOTE: 在 gotw [The Free Lunch Is Over: A Fundamental Turn Toward Concurrency in Software](http://www.gotw.ca/publications/concurrency-ddj.htm) 中 对这个问题进行了探讨

2) Python引入了coroutine

3) concurrent programming language的涌现: [golang](https://golang.org/)

总的来说，在programming language层，需要不断地融入新的语言特性来适应这种技术的演进。

**对OS的影响**

OS需要考虑如何来充分发挥CPU的parallel computing特性，比如:

1) IO multiplex

**对应用层的影响**

在应用层，需要考虑如何重复发挥CPU的parallel computing特性。

分布式计算的兴起。

## 素材

本章收录了一些讨论parallel computing发展趋势的文章:

1、preshing [A Look Back at Single-Threaded CPU Performance](https://preshing.com/20120208/a-look-back-at-single-threaded-cpu-performance/)

2、zhihu [如何理解 C++11 的六种 memory order？](https://www.zhihu.com/question/24301047) # [A](https://www.zhihu.com/question/24301047/answer/85844428) 

> 简单来说，你可以理解当代CPU不仅是多核心，而且每个核心还是多任务（多指令）并行的。计算机课本上的那种一个时钟一条指令的，早就是老黄历了 （当然，宏观来看基本原理并没有改变）