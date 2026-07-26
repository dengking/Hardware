# Align to cache line optimization



## stackoverflow [What does “cacheline aligned” mean?](https://stackoverflow.com/questions/39971639/what-does-cacheline-aligned-mean)

I read this article about PostgreSQL performance: http://akorotkov.github.io/blog/2016/05/09/scalability-towards-millions-tps/

One optimization was "cacheline aligment".

What is this? How does it help and how to apply this in code?

[A](https://stackoverflow.com/a/39992850)

CPU caches transfer data from and to main memory in chunks called [*cache lines*](https://en.wikipedia.org/wiki/CPU_cache#CACHE-LINES); a typical size for this seems to be 64 bytes.

Data that are located closer to each other than this may end up on the same cache line.

If these data are needed by different cores, the system has to work hard to keep the data consistent between the copies residing in the cores' caches. Essentially, while one thread modifies the data, the other thread is blocked by a lock from accessing the data.

The article you reference talks about one such problem that was found in PostgreSQL in a data structure in shared memory that is frequently updated by different processes. By introducing padding into the structure to inflate it to 64 bytes, it is guaranteed that no two such data structures end up in the same cache line, and the processes that access them are not blocked more that absolutely necessary.

This is only relevant if your program parallelizes execution and accesses a shared memory region, either by multithreading or by multiprocessing with shared memory. In this case you can benefit by making sure that data that are frequently accessed by different execution threads are not located close enough in memory that they can end up in the same cache line.
The typical way to do that is by adding “dead” padding space at the end of a data structure.

I found some interesting articles on the topic that you may want to read:
http://www.drdobbs.com/parallel/maximize-locality-minimize-contention/208200273?pgno=3
http://www.drdobbs.com/tools/memory-constraints-on-thread-performance/231300494
http://www.drdobbs.com/parallel/eliminate-false-sharing/217500206

## drdobbs 系列

http://www.drdobbs.com/parallel/maximize-locality-minimize-contention/208200273?pgno=3
http://www.drdobbs.com/tools/memory-constraints-on-thread-performance/231300494
http://www.drdobbs.com/parallel/eliminate-false-sharing/217500206

上述文章现在都无法访问了。