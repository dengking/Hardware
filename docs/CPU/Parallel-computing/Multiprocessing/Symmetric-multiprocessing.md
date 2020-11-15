# SMP: Symmetric multiprocessing

"symmetric"的含义是"对称"。

## wikipedia [Symmetric multiprocessing](https://en.wikipedia.org/wiki/Symmetric_multiprocessing)

**Symmetric multiprocessing** (**SMP**) involves a [multiprocessor](https://en.wikipedia.org/wiki/Multiprocessor) computer hardware and software architecture where two or more identical processors are connected to a single, shared [main memory](https://en.wikipedia.org/wiki/Main_memory), have full access to all input and output devices, and are controlled by a single operating system instance that treats all processors equally, reserving none for special purposes. Most multiprocessor systems today use an **SMP architecture**. In the case of [multi-core processors](https://en.wikipedia.org/wiki/Multi-core_processor), the SMP architecture applies to the cores, treating them as separate processors.

> NOTE: 这是当今主流的architecture。可以使用multiple model来描述SMP architecture，参见工程parallel-computing的`Model\Multiple-model`章节，因此，当采用SMP architecture的时候，就会面临multiple model中描述的所有的问题。



SMP systems are *tightly coupled multiprocessor systems* with a pool of homogeneous(同构的) processors running independently of each other. Each processor, executing different programs and working on different sets of data, has the capability of sharing **common resources** (**memory**, I/O device, interrupt system and so on) that are connected using a [system bus](https://en.wikipedia.org/wiki/System_bus) or a [crossbar](https://en.wikipedia.org/wiki/Crossbar_switch).

[![img](https://upload.wikimedia.org/wikipedia/commons/thumb/1/1c/SMP_-_Symmetric_Multiprocessor_System.svg/440px-SMP_-_Symmetric_Multiprocessor_System.svg.png)](https://en.wikipedia.org/wiki/File:SMP_-_Symmetric_Multiprocessor_System.svg)

Diagram of a symmetric multiprocessing system



### Design

SMP systems have **centralized** [shared memory](https://en.wikipedia.org/wiki/Shared_memory_architecture) called *main memory* (MM) operating under a single [operating system](https://en.wikipedia.org/wiki/Operating_system) with two or more homogeneous processors. Usually each processor has an associated private high-speed memory known as [cache memory](https://en.wikipedia.org/wiki/Cache_memory) (or cache) to speed up the main memory data access and to reduce the system bus traffic.

> NOTE: 从上面的图可以看出，每个processor都有自己的cache，上面这段话介绍了，这样做的目的是speed up main memory data access。

Processors may be interconnected(互联) using buses, [crossbar switches](https://en.wikipedia.org/wiki/Crossbar_switch) or on-chip mesh networks. The bottleneck in the scalability of SMP using buses or crossbar switches is the **bandwidth** and power consumption of the interconnect among the various processors, the memory, and the disk arrays. Mesh(网格) architectures avoid these bottlenecks, and provide nearly linear scalability to much higher processor counts at the sacrifice of programmability:

> Serious programming challenges remain with this kind of architecture because it requires two distinct modes of programming; one for the CPUs themselves and one for the interconnect between the CPUs. A single programming language would have to be able to not only partition the workload, but also comprehend the memory locality, which is severe in a mesh-based architecture.[[3\]](https://en.wikipedia.org/wiki/Symmetric_multiprocessing#cite_note-AutoMQ-1-3)

SMP systems allow any processor to work on any task no matter where the data for that task is located in memory, provided that each task in the system is not in execution on two or more processors at the same time. With proper [operating system](https://en.wikipedia.org/wiki/Operating_system) support, **SMP systems** can easily move tasks between processors to balance the workload（工作负载） efficiently.

### Performance

When more than one program executes at the same time, an SMP system has considerably better performance than a uni-processor, because different programs can run on different CPUs simultaneously.

In cases where an SMP environment processes many jobs, administrators often experience a loss of hardware efficiency. Software programs have been developed to schedule jobs so that the processor utilization reaches its maximum potential. Good software packages can achieve this maximum potential by scheduling each CPU separately, as well as being able to integrate multiple SMP machines and clusters.

**Access to RAM is serialized**; this and [cache coherency](https://infogalactic.com/info/Cache_coherency) issues causes performance to lag(落后) slightly behind the number of additional processors in the system.

> NOTE: 这种结构的瓶颈所在。结构决定***。



## Consistency model of SMP

这个问题在Wikipedia [Memory ordering](https://infogalactic.com/info/Memory_ordering)中进行了讨论:

> ### In symmetric multiprocessing (SMP) microprocessor systems
>
> There are several memory-consistency models for [SMP](https://infogalactic.com/info/Symmetric_multiprocessing) systems:
>
> - Sequential consistency (all reads and all writes are in-order)
> - Relaxed consistency (some types of reordering are allowed)
>   - Loads can be reordered after loads (for better working of cache coherency, better scaling)
>   - Loads can be reordered after stores
>   - Stores can be reordered after stores
>   - Stores can be reordered after loads
> - Weak consistency (reads and writes are arbitrarily reordered, limited only by explicit [memory barriers](https://infogalactic.com/info/Memory_barrier))