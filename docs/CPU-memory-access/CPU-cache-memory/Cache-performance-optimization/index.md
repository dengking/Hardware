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

1、align-to-cache line-optimization

2、线程定位来避免 cache sloshing-晃动

典型代表就是jemalloc

3、abseil B-tree Container-use array to cache optimization

4、padding-to-cache line-optimization-avoid false sharing



## TODO

[How to improve my cache hit rate?](https://support.ezoic.com/kb/article/how-to-improve-my-cache-hit-rate)