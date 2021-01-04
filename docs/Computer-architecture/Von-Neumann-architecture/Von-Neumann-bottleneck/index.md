# Von Neumann bottleneck

## Von Neumann bottleneck: "限制CPU速度的是从内存中读写数据"

记得大学时在学习**计算机组成原理**课程的时候，老师提出过重要的观点：

“限制CPU速度的是从内存中读写数据”

意思是CPU的ALU的运算速度是非常快的，相比之下从内存中读取是比较缓慢的，所以ALU常常需要等待，这应该是当代CPU设计时需要考虑的一个矛盾所在，各种缓解这个矛盾的技术不断出现，比如: 

1) 在Book-计算机组成原理-科学出版社的5.1.3CPU中的主要寄存器章节中所描述的**数据缓冲寄存器（DR）**的作用：补偿CPU和内存、外围设备之间在操作速度上的差别。

本章主要介绍CPU访问memory相关的内容，读者记住上面的这个观点，应该能够有助于理解使用一些技术的目的所在。

> NOTE: 这其实是 [von Neumann bottleneck](https://en.wikipedia.org/wiki/Von_Neumann_architecture#Von_Neumann_bottleneck)  ，参见`Computer-architecture\Von-Neumann-architecture`章节。

## wikipedia [Von Neumann architecture](https://en.wikipedia.org/wiki/Von_Neumann_architecture) # Design limitations # Von Neumann bottleneck



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


