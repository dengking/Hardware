# gotw [The Free Lunch Is Over: A Fundamental Turn Toward Concurrency in Software](http://www.gotw.ca/publications/concurrency-ddj.htm)

> NOTE: 
>
> 1、是在阅读 preshing [A Look Back at Single-Threaded CPU Performance](https://preshing.com/20120208/a-look-back-at-single-threaded-cpu-performance/) 时，发现的这篇文章
>
> 2、在 [更好的内存管理-jemalloc (redis 默认使用的)](https://blog.csdn.net/weixin_34357436/article/details/92090661) 中，对这篇文章的核心观点进行了非常好的解读:
>
> > 2005年发表了一篇文章“[免费午餐的时代结束了](http://www.gotw.ca/publications/concurrency-ddj.htm)”。在之前，程序就算不用费脑子，随着cpu时钟速度增加，程序性能自己就会上去。但现在不同，现在cpu时钟趋于稳定，而核数不断地增加。程序员需要适应这样的多线程多进程的环境，并要开发出适合的程序。文章讲的大概是这样的内容。
> >
> > 6年之后的如今，这篇文章完全变成现实了。事实上cpu时钟停留在3GHz，而核不断上升。现在程序要适应多线程多进程的分布式计算，速度才能上升。但是这样的程序很难。
>
> 



## 序言

**By Herb Sutter**

The biggest sea change in **software development** since the **OO revolution** is knocking at the door, and its name is **Concurrency**.

*This article appeared in **[Dr. Dobb's Journal](http://www.ddj.com/), 30(3), March 2005**. A much briefer version under the title **"The Concurrency Revolution"** appeared in **[C/C++ Users Journal](http://www.cuj.com/), 23(2), February 2005**.*

**Update note: The CPU trends graph last updated August 2009 to include current data and show the trend continues as predicted. The rest of this article including all text is still original as first posted here in December 2004.**

---

Your free lunch will soon be over. What can you do about it? What *are* you doing about it?

The major processor manufacturers and architectures, from Intel and AMD to Sparc and PowerPC, have run out of room with most of their traditional approaches to boosting CPU performance. Instead of driving(驱动) clock speeds and straight-line instruction throughput ever higher, they are instead turning *en masse*(一同地) to hyperthreading(超线程) and multicore architectures. Both of these features are already available on chips today; in particular, multicore is available on current PowerPC and Sparc IV processors, and is coming in 2005 from Intel and AMD. Indeed, the big theme of the 2004 In-Stat/MDR Fall Processor Forum was multicore devices, as many companies showed new or updated multicore processors. Looking back, it’s not much of a stretch to call 2004 the year of multicore.

And that puts us at a fundamental turning point in software development, at least for the next few years and for applications targeting general-purpose desktop computers and low-end servers (which happens to account for the vast bulk of the dollar value of software sold today). In this article, I’ll describe the changing face of hardware, why it suddenly does matter to software, and how specifically the concurrency revolution matters to you and is going to change the way you will likely be writing software in the future.

Arguably, the free lunch has already been over for a year or two, only we’re just now noticing.