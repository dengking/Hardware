# Memory coherence

"coherence"的意思是"一致性"。

## Wikipedia [Memory coherence](https://infogalactic.com/info/Memory_coherence)

**Memory coherence** is an issue that affects the design of [computer systems](https://infogalactic.com/info/Computer_system) in which two or more [processors](https://infogalactic.com/info/Central_processing_unit) or [cores](https://infogalactic.com/info/Multi_core) share a common area of [memory](https://infogalactic.com/info/Memory_(computers)).[[1\]](https://infogalactic.com/info/Memory_coherence#cite_note-censier98-1)[[2\]](https://infogalactic.com/info/Memory_coherence#cite_note-smith82-2)[[3\]](https://infogalactic.com/info/Memory_coherence#cite_note-li89-3)[[4\]](https://infogalactic.com/info/Memory_coherence#cite_note-stenstrom90-4)

> NOTE:  从one([uniprocessor](https://infogalactic.com/info/Uniprocessor)) to many( [multiprocessor](https://infogalactic.com/info/Multiprocessor) (or [multicore](https://infogalactic.com/info/Multi_core)) )，复杂度提升了非常多，因此需要相应的理论来对这种情况进行说明。

In a [uniprocessor](https://infogalactic.com/info/Uniprocessor) system (whereby, in today's terms, there exists only one core), there is only one processing element doing all the work and therefore only one processing element that can read or write from/to a given memory location. As a result, when a value is changed, all subsequent read operations of the corresponding memory location will see the updated value, even if it is [cached](https://infogalactic.com/info/Cache_(computing)).

> NOTE: [uniprocessor](https://infogalactic.com/info/Uniprocessor) 是非常简单的；

Conversely, in [multiprocessor](https://infogalactic.com/info/Multiprocessor) (or [multicore](https://infogalactic.com/info/Multi_core)) systems, there are two or more **processing elements**(对应的是multiple model中的entity) working at the same time, and so it is possible that they simultaneously access the same memory location. Provided none of them changes the data in this location, they can share it indefinitely and cache it as they please. But as soon as one updates the location, the others might work on an **out-of-date** copy that, e.g., resides in their local cache. Consequently, some scheme is required to notify all the **processing elements** of changes to **shared values**; such a scheme is known as a "**memory coherence protocol**", and if such a protocol is employed the system is said to have a "**coherent memory**".

> NOTE: 使用multiple model进行描述。

The exact nature and meaning of the memory coherency is determined by the [consistency model](https://infogalactic.com/info/Consistency_model) that the coherence protocol implements. In order to write correct concurrent programs, programmers must be aware of the exact consistency model that is employed by their systems.

> NOTE: 可以使用consistency model来描述这种问题

When implemented in hardware, the **coherency protocol** can, e.g., be [directory based](https://infogalactic.com/info/Directory-based_coherence_protocols) or employ [snooping](https://infogalactic.com/info/Bus_sniffing) (a.k.a. "sniffing"). Examples of specific protocols are the [MSI protocol](https://infogalactic.com/info/MSI_protocol) and its derivatives [MESI](https://infogalactic.com/info/MESI_protocol), [MOSI](https://infogalactic.com/info/MOSI_protocol) and [MOESI](https://infogalactic.com/info/MOESI_protocol).