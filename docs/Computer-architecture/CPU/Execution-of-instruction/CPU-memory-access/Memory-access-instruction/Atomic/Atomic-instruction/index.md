# 关于本章

本章标题"instruction sequence"的含义是指令序列，它是指通过特定的指令序列来实现concurrency control的方式。

## 实现思路

依据multiple model，我们可知，对memory的operation可以分为两大类:

1、read

2、write

atomic operation的实现思路是典型的"assemble as atomic primitive": 将可能的read、write 操作进行**集成**，提供实现**集成功能**的atomic instruction，典型的例子包括:

1、Test-and-set

2、Fetch-and-add

3、Compare-and-swap



## Atomic operation cost

这是在阅读stackoverflow [Why would I std::move an std::shared_ptr?](https://stackoverflow.com/questions/41871115/why-would-i-stdmove-an-stdshared-ptr) 时，其中提及的:

> @xaviersjs The assignment requires an atomic increment followed by an atomic decrement when Value goes out of scope. Atomic operations can take hundreds of clock cycles. So yes, it really is that much slower. – [Adisak](https://stackoverflow.com/users/14904/adisak)
>
>  @Adisak that's the first I've heard the fetch and add operation ([en.wikipedia.org/wiki/Fetch-and-add](https://en.wikipedia.org/wiki/Fetch-and-add)) could take hundreds of cycles more than a basic increment. Do you have a reference for that? – [xaviersjs](https://stackoverflow.com/users/1196033/xaviersjs) 
>
> @xaviersjs : [stackoverflow.com/a/16132551/4238087](https://stackoverflow.com/a/16132551/4238087) With register operations being a few cycles, 100's (100-300) of cycles for atomic fits the bill. Although metrics are from 2013, this still seems to be true especially for multi-socket NUMA systems. – [russianfool](https://stackoverflow.com/users/4238087/russianfool)
>
>  

 

### stackoverflow [atomic operation cost](https://stackoverflow.com/questions/2538070/atomic-operation-cost)