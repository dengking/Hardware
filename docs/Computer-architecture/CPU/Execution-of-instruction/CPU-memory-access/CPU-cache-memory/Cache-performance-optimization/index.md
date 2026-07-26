# Cache performance optimization



## How Cache Coherency Impacts Power Performance

1、Cache Coherency是有代价的，它会对Power、Performance等产生重要影响

2、我是在阅读 csdn [聊聊高并发（五）理解缓存一致性协议以及对并发编程的影响](https://blog.csdn.net/iter_zc/article/details/40342695) 时，联想到这个topic的，其中讨论了cache coherency对concurrency的影响

其中提及了"cache coherence flood"概念。

3、本章的标题源自 semiengineering [How Cache Coherency Impacts Power, Performance](https://semiengineering.com/how-cache-coherency-impacts-power-performance/)

### semiengineering [How Cache Coherency Impacts Power, Performance](https://semiengineering.com/how-cache-coherency-impacts-power-performance/)



## Cache performance optimization

cache performance 影响方方面面，无论是concurrency、single thread等，它是optimization重要方向；

下面是我目前遇到过的一些optimization方式: 

一、align-to-cache line-optimization

二、避免 "cache sloshing-晃动"

在 jemalloc的paper [A Scalable Concurrent malloc(3) Implementation for FreeBSD](https://people.freebsd.org/~jasone/jemalloc/bsdcan2006/jemalloc.pdf) 中，对"cache sloshing-晃动"的解释为:

"They attributed this to “cache sloshing” – the quick migration of cached data among processors during the manipulation of allocator data structures. "

其实就是 "cache coherence flood-broadcast-bus traffic-interconnect contention-memory synchronization多核同步内存"。

实现方式: 

1、线程定位来避免"cache sloshing-晃动"

典型代表就是jemalloc

2、使用concurrency-friendly data structure

参见 drdobbs [Choose Concurrency-Friendly Data Structures](https://www.drdobbs.com/parallel/choose-concurrency-friendly-data-structu/208801371)



三、abseil B-tree Container-use array to cache optimization

四、padding-to-cache line-optimization-avoid false sharing



### Array

1、相比于linked list，array的cache performance是更好的、cache hit rate也是更高的

2、multiple dimension array的cache hit rate是高于nested vector(`std::vector<std::vector<int>>`)的



## TODO

[How to improve my cache hit rate?](https://support.ezoic.com/kb/article/how-to-improve-my-cache-hit-rate)