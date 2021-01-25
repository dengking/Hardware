# gotw [The Free Lunch Is Over: A Fundamental Turn Toward Concurrency in Software](http://www.gotw.ca/publications/concurrency-ddj.htm)

> NOTE: 是在阅读 preshing [A Look Back at Single-Threaded CPU Performance](https://preshing.com/20120208/a-look-back-at-single-threaded-cpu-performance/) 时，发现的这篇文章



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