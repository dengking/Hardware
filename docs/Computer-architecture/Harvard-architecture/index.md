
# Harvard architecture


## wikipedia [Harvard architecture](https://en.wikipedia.org/wiki/Harvard_architecture)

The **Harvard architecture** is a [computer architecture](https://en.wikipedia.org/wiki/Computer_architecture) with physically separate [storage](https://en.wikipedia.org/wiki/Computer_storage) and signal pathways for [instructions](https://en.wikipedia.org/wiki/Instruction_(computing)) and data. The term originated from the [Harvard Mark I](https://en.wikipedia.org/wiki/Harvard_Mark_I) relay-based computer, which stored instructions on [punched tape](https://en.wikipedia.org/wiki/Punched_tape) (24 bits wide) and data in [electro-mechanical](https://en.wikipedia.org/wiki/Electro-mechanical) counters. These early machines had data storage entirely contained within the [central processing unit](https://en.wikipedia.org/wiki/Central_processing_unit), and provided no access to the instruction storage as data. Programs needed to be loaded by an operator; the processor could not [initialize](https://en.wikipedia.org/wiki/Booting) itself.

***TRANSLATION*** : 哈佛体系结构是一种计算机体系结构，它为指令和数据分别提供了物理上独立的存储和信号通路。这个术语起源于基于哈佛Mark I中继的计算机，它将指令存储在穿孔带(24位宽)上，并将数据存储在机电计数器中。这些早期机器的数据存储完全包含在中央处理单元中，不提供对指令存储作为数据的访问。操作人员需要加载的程序;处理器无法初始化自己。

***SUMMARY*** : 第一句话就就点明了Harvard architecture与Von Neumann architecture最大的差别，它在于如何对待instruction 和 data；Von Neumann architecture并不区分instruction和data，它将它们都视为data；而Harvard architecture则严格区分两者；



Today, most processors implement such **separate signal pathways** for performance reasons, but actually implement a [modified Harvard architecture](https://en.wikipedia.org/wiki/Modified_Harvard_architecture), so they can support tasks like loading a program from [disk storage](https://en.wikipedia.org/wiki/Disk_storage) as data and then executing it.

![img](https://upload.wikimedia.org/wikipedia/commons/thumb/3/3f/Harvard_architecture.svg/220px-Harvard_architecture.svg.png)





Harvard architecture

## wikipedia [Modified Harvard architecture](https://en.wikipedia.org/wiki/Modified_Harvard_architecture)

The **modified Harvard architecture** is a variation of the [Harvard computer architecture](https://en.wikipedia.org/wiki/Harvard_architecture) that allows the contents of the **instruction memory** to be accessed as if it were **data**. Most modern computers that are documented as Harvard architecture are, in fact, modified Harvard architecture.

***SUMMARY*** : 显然modified Harvard architeture吸收了Von Neumann architecture的并不区分instruction和data，将它们都视为data 的思想；



### Harvard architecture

Main article: [Harvard architecture](https://en.wikipedia.org/wiki/Harvard_architecture)

The original Harvard architecture computer, the [Harvard Mark I](https://en.wikipedia.org/wiki/Harvard_Mark_I), employed entirely separate memory systems to store instructions and data. The [CPU](https://en.wikipedia.org/wiki/CPU) fetched the next instruction and loaded or stored data **simultaneously** and independently（有些parallel的思想）. This is in contrast to a [von Neumann architecture](https://en.wikipedia.org/wiki/Von_Neumann_architecture) computer, in which both instructions and data are stored in the same memory system and (without the complexity of a [CPU cache](https://en.wikipedia.org/wiki/CPU_cache)) must be accessed **in turn**. The physical separation of instruction and data memory is sometimes held to be the distinguishing feature of modern Harvard architecture computers. With [microcontrollers](https://en.wikipedia.org/wiki/Microcontroller) (entire computer systems integrated onto single chips), the use of different memory technologies for instructions (e.g. [flash memory](https://en.wikipedia.org/wiki/Flash_memory)) and data (typically [read/write memory](https://en.wikipedia.org/wiki/Read-write_memory)) in von Neumann machines is becoming popular（利用微控制器（整个计算机系统集成到单个芯片上），在冯·诺依曼机器中使用不同的存储器技术用于指令（例如闪存）和数据（通常是读/写存储器）正变得流行）. The true distinction of a Harvard machine is that instruction and data memory occupy different [address spaces](https://en.wikipedia.org/wiki/Address_space). In other words, a memory address does not uniquely identify a storage location (as it does in a von Neumann machine); it is also necessary to know the memory space (instruction or data) to which the address belongs.



### Von Neumann architecture

Main article: [Von Neumann architecture](https://en.wikipedia.org/wiki/Von_Neumann_architecture)

A computer with a von Neumann architecture has the advantage over *pure* Harvard machines in that code can also be accessed and treated the same as data, and vice versa. This allows, for example, data to be read from [disk storage](https://en.wikipedia.org/wiki/Disk_storage) into memory and then executed as code, or self-optimizing software systems using technologies such as [just-in-time compilation](https://en.wikipedia.org/wiki/Just-in-time_compilation) to write machine code into their own memory and then later execute it. Another example is [self-modifying code](https://en.wikipedia.org/wiki/Self-modifying_code), which allows a program to modify itself. A disadvantage of these methods are issues with [executable space protection](https://en.wikipedia.org/wiki/Executable_space_protection), which increase the risks from [malware](https://en.wikipedia.org/wiki/Malware) and software defects. In addition, in these systems it is notoriously difficult to document code flow, and also can make debugging much more difficult.