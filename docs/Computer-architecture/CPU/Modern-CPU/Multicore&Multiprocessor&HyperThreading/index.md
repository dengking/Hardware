# Multicore VS Multiprocessor

## pediaa [Difference Between Multicore and Multiprocessor](https://pediaa.com/difference-between-multicore-and-multiprocessor/)

The **main difference** between multicore and multiprocessor is that the **multicore refers to a single CPU with multiple execution units while the multiprocessor refers to a system that has two or more CPUs.**



## superuser [What's the difference between multicore proc and multiproc system?](https://superuser.com/questions/13107/whats-the-difference-between-multicore-proc-and-multiproc-system)

[A](https://superuser.com/a/13418)

Multiple processors let your computer do literally two things at once (instead of only seemingly doing two things at once, but actually just swapping between tasks extremely rapidly).

**Multiple cores** are the same. The advantage of multiple cores over multiple processors is that they share some bits of the CPU, e.g. the second level cache, which makes it possible for them to work even more efficiently if they have some shared data. This makes them much cheaper to manufacture. A single dual-core CPU also takes up less room than two single-core CPUs, which is an important factor these days with everyone moving to laptops.

> NOTE: 
>
> 一、multiple core 相较于 multiple processor的优势:
>
> 1、share L2 cache，more efficient
>
> 2、more cheaper to manufacture
>
> 3、take less room

There may be some performance differences, but nothing you're likely to notice.



[A](https://superuser.com/a/214361)

See this image that shows the difference between **Multi Processor**, **Hyper Threaded** and **Multi Core**:

![enter image description here](https://i.stack.imgur.com/STuQI.png)

> NOTE: 总结地非常好



##  superuser [What is the difference between MultiCore and MultiProcessor?](https://superuser.com/questions/214331/what-is-the-difference-between-multicore-and-multiprocessor)



