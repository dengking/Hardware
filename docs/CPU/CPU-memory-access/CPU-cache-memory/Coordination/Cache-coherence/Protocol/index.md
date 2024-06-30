# 关于本章



## wikipedia [Cache coherence](https://infogalactic.com/info/Cache_coherence) # Coherency protocols

[Distributed shared memory](https://infogalactic.com/info/Distributed_shared_memory) systems employ *coherency protocols* to ensure the consistency between all caches, by maintaining [memory coherence](https://infogalactic.com/info/Memory_coherence) according to a specific [consistency model](https://infogalactic.com/info/Consistency_model). 

Older multiprocessors support the [sequential consistency](https://infogalactic.com/info/Sequential_consistency) model, while modern shared memory systems typically support the [release consistency](https://infogalactic.com/info/Release_consistency) or [weak consistency](https://infogalactic.com/info/Weak_consistency) models.

The protocol must implement the basic requirements for coherence. It can be tailor-made for the target system or application.

Protocols can also be classified as **snoopy** or **directory-based**. Typically, early systems used **directory-based protocols** where a directory would keep a track of the data being shared and the sharers. In **snoopy protocols**, the transaction requests (to read, write, or upgrade) are sent out to all processors. All processors snoop the request and respond appropriately.

## write policy: write-through and write-back

### stackoverflow [Write-back vs Write-Through caching?](https://stackoverflow.com/questions/27087912/write-back-vs-write-through-caching)

[A](https://stackoverflow.com/a/57367793/10173843)

> NOTE: 
>
> 这个回答非常简洁明了

Hope this article can help you [Differences between disk Cache Write-through and Write-back](https://forum.huawei.com/enterprise/en/differences-between-disk-cache-write-through-and-write-back/thread/203781-891)

Write-through: Write is done synchronously both to the **cache** and to the **backing store**.

> NOTE: 
>
> "synchronously"这个词用的非常好

Write-back (or Write-behind): Writing is done only to the cache. A modified cache block is written back to the store, just before it is replaced.

> NOTE: 
>
> 只有当"modified cache block"被驱逐出cache的时候，它才会被写入奥backing store

Write-through: When data is updated, it is written to both the cache and the back-end storage. This mode is easy for operation but is slow in data writing because data has to be written to both the cache and the storage.

Write-back: When data is updated, it is written only to the cache. The modified data is written to the back-end storage only when data is removed from the cache. This mode has fast data write speed but data will be lost if a power failure occurs before the updated data is written to the storage.

> NOTE: 
>
> 一、
>
> write-through: 简单但是慢
>
> write-back: 复杂但是快

[A](https://stackoverflow.com/a/27161893/10173843)

> NOTE: 
>
> 这个回答是深入浅出的，它还涉及了一些其它的知识

### wikipedia [Cache hierarchy # Write policies](https://en.wikipedia.org/wiki/Cache_hierarchy#Write_policies)

There are two policies which define the way in which a modified cache block will be updated in the main memory: write through and write back.



### tau [Multiprocessor Programming](http://www.cs.tau.ac.il/~shanir/multiprocessor-synch-2003/) # [Lecture 7: Spin Locks and Contention Management](http://www.cs.tau.ac.il/~shanir/multiprocessor-synch-2003/spin/notes/spin.pdf)

> NOTE: 
>
> 1、其中的"7.3 Cache Memory & Consistency"中讨论了write back (回写)、write-through (直写)





### TODO

下面是一些非常好的文章:

shahriar.svbtle [Understanding write-through, write-around and write-back caching (with Python)](https://shahriar.svbtle.com/Understanding-writethrough-writearound-and-writeback-caching-with-python)



## 各种 [cache coherency](https://en.wikipedia.org/wiki/Cache_coherency) protocol

本章讨论各种一些[cache coherency](https://en.wikipedia.org/wiki/Cache_coherency) protocol：

| protocols      | states                                                       | 章节 |
| -------------- | ------------------------------------------------------------ | ---- |
| MSI protocol   | **M**odified、**S**hared、**I**nvalid                        |      |
| MESI protocol  | **M**odified、**E**xclusive、**S**hared、**I**nvalid         |      |
| MOESI protocol | **M**odified、**O**wned、**E**xclusive、**S**hared、**I**nvalid |      |

可以看到，这些protocol的差异主要在于state数的不同。

