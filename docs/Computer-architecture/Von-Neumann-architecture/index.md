# Von Neumann architecture



## wikipedia [Von Neumann architecture](https://en.wikipedia.org/wiki/Von_Neumann_architecture)

See also: [Stored-program computer](https://en.wikipedia.org/wiki/Stored-program_computer) and [Universal Turing machine § Stored-program computer](https://en.wikipedia.org/wiki/Universal_Turing_machine#Stored-program_computer)

The **von Neumann architecture**—also known as the **von Neumann model** or **Princeton architecture**—is a [computer architecture](https://en.wikipedia.org/wiki/Computer_architecture) based on a 1945 description by the mathematician and physicist [John von Neumann](https://en.wikipedia.org/wiki/John_von_Neumann) and others in the *First Draft of a Report on the EDVAC*.[[1\]](https://en.wikipedia.org/wiki/Von_Neumann_architecture#cite_note-FirstDraftReport-1) That document describes a design architecture for an electronic [digital computer](https://en.wikipedia.org/wiki/Digital_computer) with these components:

- A [processing unit](https://en.wikipedia.org/wiki/Central_processing_unit) that contains an [arithmetic logic unit](https://en.wikipedia.org/wiki/Arithmetic_logic_unit) and [processor registers](https://en.wikipedia.org/wiki/Processor_register)

  > NOTE: 上面所说的processing unit指的就是CPU

- A [control unit](https://en.wikipedia.org/wiki/Control_unit) that contains an [instruction register](https://en.wikipedia.org/wiki/Instruction_register) and [program counter](https://en.wikipedia.org/wiki/Program_counter)

- [Memory](https://en.wikipedia.org/wiki/Computer_memory) that stores [data](https://en.wikipedia.org/wiki/Data_(computing)) and [instructions](https://en.wikipedia.org/wiki/Instruction_set)

  > NOTE: 要充分理解上面这段话的含义，就需要阅读See also: [Stored-program computer](https://en.wikipedia.org/wiki/Stored-program_computer) and [Universal Turing machine § Stored-program computer](https://en.wikipedia.org/wiki/Universal_Turing_machine#Stored-program_computer)

- External [mass storage](https://en.wikipedia.org/wiki/Mass_storage)

- [Input and output](https://en.wikipedia.org/wiki/Input_and_output) mechanisms[[1\]](https://en.wikipedia.org/wiki/Von_Neumann_architecture#cite_note-FirstDraftReport-1)[[2\]](https://en.wikipedia.org/wiki/Von_Neumann_architecture#cite_note-GanesanCh4-2)

The term "von Neumann architecture" has evolved to mean any [stored-program computer](https://en.wikipedia.org/wiki/Stored-program_computer) in which an [instruction fetch](https://en.wikipedia.org/wiki/Instruction_fetch) and a data operation cannot occur at the same time because they share a common [bus](https://en.wikipedia.org/wiki/Bus_(computing)). This is referred to as the [von Neumann bottleneck](https://en.wikipedia.org/wiki/Von_Neumann_architecture#Von_Neumann_bottleneck) and often limits the performance of the system.[[3\]](https://en.wikipedia.org/wiki/Von_Neumann_architecture#cite_note-3)

> NOTE: 术语“冯·诺依曼架构”已经发展为任何存储程序计算机，其中指令获取和数据操作不能同时发生，因为它们共享公共总线。 这被称为冯诺依曼瓶颈，并且经常限制系统的性能

The design of a von Neumann architecture machine is simpler than a [Harvard architecture](https://en.wikipedia.org/wiki/Harvard_architecture) machine—which is also a **stored-program system** but has one dedicated set of address and data buses for reading and writing to memory, and another set of address and [data buses](https://en.wikipedia.org/wiki/Memory_bus) to fetch [instructions](https://en.wikipedia.org/wiki/Instruction_fetch).

A **stored-program digital computer** keeps both [program instructions](https://en.wikipedia.org/wiki/Computer_program) and data in [read-write](https://en.wikipedia.org/wiki/Read-write_memory), [random-access memory](https://en.wikipedia.org/wiki/Random-access_memory) (RAM). Stored-program computers were an advancement over the program-controlled computers of the 1940s, such as the [Colossus](https://en.wikipedia.org/wiki/Colossus_computer) and the [ENIAC](https://en.wikipedia.org/wiki/ENIAC). Those were programmed by setting switches and inserting patch cables to route data and control signals between various functional units. The vast majority of modern computers use the same memory for both data and program instructions. The von Neumann vs. Harvard distinction applies to the [cache](https://en.wikipedia.org/wiki/CPU_cache) architecture, not the main memory ([split cache architecture](https://en.wikipedia.org/wiki/Modified_Harvard_architecture#Split-cache_(or_Almost-von-Neumann)_architecture)).

![img](https://upload.wikimedia.org/wikipedia/commons/thumb/e/e5/Von_Neumann_Architecture.svg/220px-Von_Neumann_Architecture.svg.png)



Von Neumann architecture scheme



### History

The earliest computing machines had fixed programs. Some very simple computers still use this design, either for simplicity or training purposes. For example, a desk [calculator](https://en.wikipedia.org/wiki/Calculator) (in principle) is a fixed program computer. It can do basic [mathematics](https://en.wikipedia.org/wiki/Mathematics), but it cannot run a [word processor](https://en.wikipedia.org/wiki/Word_processor) or games. Changing the program of a fixed-program machine requires rewiring, restructuring, or redesigning the machine. The earliest computers were not so much "programmed" as "designed" for a particular task. "Reprogramming"—when possible at all—was a laborious process that started with [flowcharts](https://en.wikipedia.org/wiki/Flowchart) and paper notes, followed by detailed engineering designs, and then the often-arduous process of physically rewiring and rebuilding the machine. It could take three weeks to set up and debug a program on [ENIAC](https://en.wikipedia.org/wiki/ENIAC).[[4\]](https://en.wikipedia.org/wiki/Von_Neumann_architecture#cite_note-4)

> NOTE: 最早的计算机有固定的程序。一些非常简单的计算机仍然使用这种设计，无论是为了简单还是培训。例如，桌面计算器（原则上）是固定程序计算机。它可以做基础数学，但它不能运行文字处理器或游戏。更改固定程序机器的程序需要重新布线，重组或重新设计机器。最早的计算机并没有为特定任务“编程”为“设计”。 “重新编程” - 在可能的情况下 - 是一个艰苦的过程，从流程图和纸质笔记开始，然后是详细的工程设计，然后是经常艰苦的物理重新布线和重建机器的过程。在ENIAC上设置和调试程序可能需要三周时间。[4]

With the proposal of the **stored-program computer**, this changed. A stored-program computer includes, by design, an [instruction set](https://en.wikipedia.org/wiki/Instruction_set), and can store in memory a set of instructions (a [program](https://en.wikipedia.org/wiki/Computer_program)) that details the [computation](https://en.wikipedia.org/wiki/Computation).

> NOTE: 随着存储程序计算机的提议，这改变了。存储程序计算机通过设计包括指令集，并且可以在存储器中存储详细说明计算的一组指令（程序）。



A **stored-program design** also allows for [self-modifying code](https://en.wikipedia.org/wiki/Self-modifying_code). One early motivation for such a facility was the need for a program to increment or otherwise modify the address portion of instructions, which operators had to do manually in early designs. This became less important when [index registers](https://en.wikipedia.org/wiki/Index_register) and [indirect addressing](https://en.wikipedia.org/wiki/Addressing_mode) became usual features of machine architecture. Another use was to embed frequently used data in the instruction stream using [immediate addressing](https://en.wikipedia.org/wiki/Addressing_mode). Self-modifying code has largely fallen out of favor, since it is usually hard to understand and [debug](https://en.wikipedia.org/wiki/Debugging), as well as being inefficient under modern processor [pipelining](https://en.wikipedia.org/wiki/Pipeline_(computing)) and caching schemes.

> NOTE: 存储程序设计还允许自修改代码。这种设施的一个早期动机是需要一个程序来增加或以其他方式修改指令的地址部分，操作员必须在早期设计中手动完成。当索引寄存器和间接寻址成为机器架构的常用功能时，这变得不那么重要了。另一种用途是使用立即寻址将经常使用的数据嵌入指令流中。自修改代码已大部分失宠，因为它通常很难理解和调试，并且在现代处理器流水线和缓存方案下效率低下。



### Capabilities

On a large scale, the ability to treat **instructions** as **data** is what makes [assemblers](https://en.wikipedia.org/wiki/Assembly_language#Assembler), [compilers](https://en.wikipedia.org/wiki/Compiler), [linkers](https://en.wikipedia.org/wiki/Linker_(computing)), [loaders](https://en.wikipedia.org/wiki/Loader_(computing)), and other automated programming tools possible. It makes "programs that write programs" possible.[[5\]](https://en.wikipedia.org/wiki/Von_Neumann_architecture#cite_note-5) This has made a sophisticated self-hosting computing ecosystem flourish around von Neumann architecture machines.

> NOTE: 在很大程度上，将指令视为数据的能力使得汇编器，编译器，链接器，加载器和其他自动编程工具成为可能。 它使“编写程序的程序”成为可能。[5] 这使得复杂的自托管计算生态系统在冯·诺依曼架构机器中蓬勃发展。

Some high level languages leverage the von Neumann architecture by providing an abstract, machine-independent way to manipulate executable code at runtime (e.g., [LISP](https://en.wikipedia.org/wiki/LISP)), or by using runtime information to tune [just-in-time compilation](https://en.wikipedia.org/wiki/Just-in-time_compilation) (e.g. languages hosted on the [Java virtual machine](https://en.wikipedia.org/wiki/Java_virtual_machine), or languages embedded in [web browsers](https://en.wikipedia.org/wiki/Web_browsers)).

On a smaller scale, some repetitive operations such as [BITBLT](https://en.wikipedia.org/wiki/Bit_blit) or [pixel and vertex shaders](https://en.wikipedia.org/wiki/High-level_shader_language) can be accelerated on general purpose processors with just-in-time compilation techniques. This is one use of self-modifying code that has remained popular.



### Development of the stored-program concept

The mathematician [Alan Turing](https://en.wikipedia.org/wiki/Alan_Turing), who had been alerted to a problem of mathematical logic by the lectures of [Max Newman](https://en.wikipedia.org/wiki/Max_Newman) at the [University of Cambridge](https://en.wikipedia.org/wiki/University_of_Cambridge), wrote a paper in 1936 entitled *On Computable Numbers, with an Application to the Entscheidungs problem*, which was published in the *Proceedings of the London Mathematical Society*.[[6\]](https://en.wikipedia.org/wiki/Von_Neumann_architecture#cite_note-Turing1936-6) In it he described a hypothetical machine he called a *universal computing machine,* now known as the "[Universal Turing machine](https://en.wikipedia.org/wiki/Universal_Turing_machine)". The hypothetical machine had an infinite store (memory in today's terminology) that contained both instructions and data. [John von Neumann](https://en.wikipedia.org/wiki/John_von_Neumann) became acquainted with Turing while he was a visiting professor at Cambridge in 1935, and also during Turing's PhD year at the [Institute for Advanced Study](https://en.wikipedia.org/wiki/Institute_for_Advanced_Study) in [Princeton, New Jersey](https://en.wikipedia.org/wiki/Princeton,_New_Jersey) during 1936 – 1937. Whether he knew of Turing's paper of 1936 at that time is not clear.





### Evolution



Through the decades of the 1960s and 1970s computers generally became both smaller and faster, which led to evolutions in their architecture. For example, [memory-mapped I/O](https://en.wikipedia.org/wiki/Memory-mapped_I/O) lets input and output devices be treated the same as memory.[[24\]](https://en.wikipedia.org/wiki/Von_Neumann_architecture#cite_note-24) A single [system bus](https://en.wikipedia.org/wiki/System_bus) could be used to provide a modular system with lower cost[*clarification needed*]. This is sometimes called a "streamlining" of the architecture.[[25\]](https://en.wikipedia.org/wiki/Von_Neumann_architecture#cite_note-25) In subsequent decades, simple [microcontrollers](https://en.wikipedia.org/wiki/Microcontrollers) would sometimes omit features of the model to lower cost and size. Larger computers added features for higher performance.

![img](https://upload.wikimedia.org/wikipedia/commons/thumb/6/68/Computer_system_bus.svg/220px-Computer_system_bus.svg.png)





Single system bus evolution of the architecture



### Design limitations

#### Von Neumann bottleneck

> NOTE: 记得大学时在学习**计算机组成原理**课程的时候，老师提出过重要的观点：
>
> “限制CPU速度的是从内存中读写数据”
>
> 意思是CPU的ALU的运算速度是非常快的，相比之下从内存中读取是比较缓慢的，所以ALU常常需要等待，这应该是当代CPU设计时需要考虑的一个矛盾所在，各种缓解这个矛盾的技术不断出现，比如: 
>
> 1) 在Book-计算机组成原理-科学出版社的5.1.3CPU中的主要寄存器章节中所描述的**数据缓冲寄存器（DR）**的作用：补偿CPU和内存、外围设备之间在操作速度上的差别。
>
> 与Von Neumann bottleneck相关的一个问题是: IO-bound，关于此，参见工程software-engineering的`Software-analysis\Performance\Bound`章节。

The **shared bus** between the **program memory** and **data memory** leads to the *von Neumann bottleneck*, the limited [throughput](https://en.wikipedia.org/wiki/Throughput)(data transfer rate) between the [central processing unit](https://en.wikipedia.org/wiki/Central_processing_unit) (CPU) and memory compared to the amount of memory. Because the single bus can only access one of the two classes of memory at a time, throughput is lower than the rate at which the CPU can work. This seriously limits the effective processing speed when the CPU is required to perform minimal processing on large amounts of data. The CPU is continually [forced to wait](https://en.wikipedia.org/wiki/Wait_state) for needed data to move to or from memory. Since CPU speed and memory size have increased much faster than the throughput between them, the bottleneck has become more of a problem, a problem whose severity increases with every new generation of CPU.

The von Neumann bottleneck was described by [John Backus](https://en.wikipedia.org/wiki/John_Backus) in his 1977 ACM [Turing Award](https://en.wikipedia.org/wiki/Turing_Award) lecture. According to Backus:

> Surely there must be a less primitive way of making big changes in the store than by pushing vast numbers of [words](https://en.wikipedia.org/wiki/Word_(data_type)) back and forth through the von Neumann bottleneck. Not only is this tube a literal bottleneck for the data traffic of a problem, but, more importantly, it is an intellectual bottleneck that has kept us tied to word-at-a-time thinking instead of encouraging us to think in terms of the larger conceptual units of the task at hand. Thus programming is basically planning and detailing the enormous traffic of words through the von Neumann bottleneck, and much of that traffic concerns not significant data itself, but where to find it.[[26\]](https://en.wikipedia.org/wiki/Von_Neumann_architecture#cite_note-backus-26)[[27\]](https://en.wikipedia.org/wiki/Von_Neumann_architecture#cite_note-27)





#### Mitigations

There are several known methods for mitigating the Von Neumann performance bottleneck. For example, the following all can improve performance[*why?*]:

- Providing a [cache](https://en.wikipedia.org/wiki/CPU_cache) between the CPU and the [main memory](https://en.wikipedia.org/wiki/Main_memory)
- providing separate caches or separate access paths for data and instructions (the so-called [Modified Harvard architecture](https://en.wikipedia.org/wiki/Modified_Harvard_architecture))
- using [branch predictor](https://en.wikipedia.org/wiki/Branch_predictor) algorithms and logic
- providing a limited CPU stack or other on-chip [scratchpad memory](https://en.wikipedia.org/wiki/Scratchpad_memory) to reduce memory access
- Implementing the CPU and the [memory hierarchy](https://en.wikipedia.org/wiki/Memory_hierarchy) as a [system on chip](https://en.wikipedia.org/wiki/System_on_a_chip), providing greater [locality of reference](https://en.wikipedia.org/wiki/Locality_of_reference) and thus reducing latency and increasing throughput between [processor registers](https://en.wikipedia.org/wiki/Processor_register) and [main memory](https://en.wikipedia.org/wiki/Main_memory)

The problem can also be sidestepped somewhat by using [parallel computing](https://en.wikipedia.org/wiki/Parallel_computing), using for example the [non-uniform memory access](https://en.wikipedia.org/wiki/Non-uniform_memory_access) (NUMA) architecture—this approach is commonly employed by supercomputers. It is less clear whether the *intellectual bottleneck* that Backus criticized has changed much since 1977. Backus's proposed solution has not had a major influence.[*citation needed*] Modern [functional programming](https://en.wikipedia.org/wiki/Functional_programming) and [object-oriented programming](https://en.wikipedia.org/wiki/Object-oriented_programming) are much less geared towards "pushing vast numbers of words back and forth" than earlier languages like [FORTRAN](https://en.wikipedia.org/wiki/FORTRAN) were, but internally, that is still what computers spend much of their time doing, even highly parallel supercomputers.

As of 1996, a database benchmark study found that three out of four CPU cycles were spent waiting for memory. Researchers expect that increasing the number of simultaneous instruction streams with [multithreading](https://en.wikipedia.org/wiki/Multithreading_(computer_architecture)) or single-chip [multiprocessing](https://en.wikipedia.org/wiki/Multiprocessing) will make this bottleneck even worse.[[28\]](https://en.wikipedia.org/wiki/Von_Neumann_architecture#cite_note-28) In the context of [multi-core processors](https://en.wikipedia.org/wiki/Multi-core_processor), additional [overhead](https://en.wikipedia.org/wiki/Overhead_(computing)) is required to maintain [cache coherence](https://en.wikipedia.org/wiki/Cache_coherence) between processors and threads.

### Self-modifying code

Aside from the von Neumann bottleneck, program modifications can be quite harmful, either by accident or design. In some simple stored-program computer designs, a malfunctioning program can damage itself, other programs, or the [operating system](https://en.wikipedia.org/wiki/Operating_system), possibly leading to a computer [crash](https://en.wikipedia.org/wiki/Crash_(computing)). [Memory protection](https://en.wikipedia.org/wiki/Memory_protection) and other forms of [access control](https://en.wikipedia.org/wiki/Access_control) can usually protect against both accidental and malicious program modification.