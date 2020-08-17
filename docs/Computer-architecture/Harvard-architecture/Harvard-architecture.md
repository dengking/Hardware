
# Harvard architecture


## wikipedia [Harvard architecture](https://en.wikipedia.org/wiki/Harvard_architecture)

The **Harvard architecture** is a [computer architecture](https://en.wikipedia.org/wiki/Computer_architecture) with physically separate [storage](https://en.wikipedia.org/wiki/Computer_storage) and signal pathways for [instructions](https://en.wikipedia.org/wiki/Instruction_(computing)) and data. The term originated from the [Harvard Mark I](https://en.wikipedia.org/wiki/Harvard_Mark_I) relay-based computer, which stored instructions on [punched tape](https://en.wikipedia.org/wiki/Punched_tape) (24 bits wide) and data in [electro-mechanical](https://en.wikipedia.org/wiki/Electro-mechanical) counters. These early machines had data storage entirely contained within the [central processing unit](https://en.wikipedia.org/wiki/Central_processing_unit), and provided no access to the instruction storage as data. Programs needed to be loaded by an operator; the processor could not [initialize](https://en.wikipedia.org/wiki/Booting) itself.

***TRANSLATION*** : 哈佛体系结构是一种计算机体系结构，它为指令和数据分别提供了物理上独立的存储和信号通路。这个术语起源于基于哈佛Mark I中继的计算机，它将指令存储在穿孔带(24位宽)上，并将数据存储在机电计数器中。这些早期机器的数据存储完全包含在中央处理单元中，不提供对指令存储作为数据的访问。操作人员需要加载的程序;处理器无法初始化自己。

***SUMMARY*** : 第一句话就就点明了Harvard architecture与Von Neumann architecture最大的差别，它在于如何对待instruction 和 data；Von Neumann architecture并不区分instruction和data，它将它们都视为data；而Harvard architecture则严格区分两者；



Today, most processors implement such **separate signal pathways** for performance reasons, but actually implement a [modified Harvard architecture](https://en.wikipedia.org/wiki/Modified_Harvard_architecture), so they can support tasks like loading a program from [disk storage](https://en.wikipedia.org/wiki/Disk_storage) as data and then executing it.

![img](https://upload.wikimedia.org/wikipedia/commons/thumb/3/3f/Harvard_architecture.svg/220px-Harvard_architecture.svg.png)





Harvard architecture