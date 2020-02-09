# Memory access

processor和memory之间的数据传输是processor的主要活动之一，本章主要对与此相关的内容进行整理，下面是涉及到的一些内容。

## [Memory–processor transfer](https://en.wikipedia.org/wiki/Word_(computer_architecture)#Uses_of_words)

When the processor reads from the memory subsystem into a register or writes a register's value to memory, the amount of data transferred is often a **word**. Historically, this amount of bits which could be transferred in one cycle was also called a *catena* in some environments. In simple memory subsystems, the word is transferred over the memory [data bus](https://en.wikipedia.org/wiki/Bus_(computing)), which typically has a width of a word or half-word. In memory subsystems that use [caches](https://en.wikipedia.org/wiki/CPU_cache), the word-sized transfer is the one between the processor and the first level of cache; at lower levels of the [memory hierarchy](https://en.wikipedia.org/wiki/Memory_hierarchy) larger transfers (which are a multiple of the word size) are normally used.

上面这段话给出了Memory–processor transfer的一个简单模型，这其中会涉及一系列问题，需要进行深入分析，本章后续章节会对此进行展开。





## [Register memory architecture](https://en.wikipedia.org/wiki/Register_memory_architecture)



## [Load–store architecture](https://en.wikipedia.org/wiki/Load%E2%80%93store_architecture)