# Stored program & Von Neumann architecture & Bottleneck

本章沿着一条主线组织：**存储程序（Stored-program）思想 → 冯·诺依曼架构（Von Neumann architecture）→ 冯·诺依曼瓶颈（Von Neumann bottleneck）**。三者层层递进：存储程序是核心思想，冯·诺依曼架构是它的经典工程实现，而冯·诺依曼瓶颈则是这一实现方式带来的根本性能限制。

> **一句话定义（瓶颈）**：冯·诺依曼瓶颈指在**冯·诺依曼架构**中，CPU 与内存之间共享的**单一数据通道（总线）**成为系统性能的限制因素——**处理器再快，也要"饿着肚子"等待数据从内存搬过来**。这是理解现代计算机性能瓶颈、乃至 AI 编译器为何要做"访存优化"的**最根本的底层原因**。

---

## 一、Stored-program computer（存储程序计算机）

stored-program computer 是非常重要的思想。

### wikipedia [Stored-program computer](https://en.wikipedia.org/wiki/Stored-program_computer)

A **stored-program computer** is one which stores [program instructions](https://infogalactic.com/info/Instruction_(computer_science)) in electronic memory.[[1\]](https://infogalactic.com/info/Stored-program_computer#cite_note-1) Often the definition is extended with the requirement that the treatment of programs and data in memory be interchangeable or uniform.[[2\]](https://infogalactic.com/info/Stored-program_computer#cite_note-GilreathLaplante2003-2)[[3\]](https://infogalactic.com/info/Stored-program_computer#cite_note-Reilly2003-3)[[4\]](https://infogalactic.com/info/Stored-program_computer#cite_note-POCA-4)

A computer with a [Von Neumann architecture](https://infogalactic.com/info/Von_Neumann_architecture) stores program data and instruction data in the same memory; a computer with a [Harvard architecture](https://infogalactic.com/info/Harvard_architecture) has separate memories for storing program and data.[[5\]](https://infogalactic.com/info/Stored-program_computer#cite_note-Page2009-5)[[6\]](https://infogalactic.com/info/Stored-program_computer#cite_note-Balch2003-6)

> NOTE: von neumann architecture 和 Harvard architecture 的差异。

The stored-program computer idea can be traced back to the 1936 theoretical concept of a [universal Turing machine](https://infogalactic.com/info/Universal_Turing_machine).[[11\]](https://infogalactic.com/info/Stored-program_computer#cite_note-Copeland2006-11) Von Neumann was aware of this paper, and he impressed it on his collaborators as well.[[12\]](https://infogalactic.com/info/Stored-program_computer#cite_note-Teuscher2004-12)

> NOTE: 关于谁首先提出这个概念，历史学家追溯到了 Turing。

### wikipedia [Universal Turing machine § Stored-program computer](https://en.wikipedia.org/wiki/Universal_Turing_machine#Stored-program_computer)

### Function and data model

[Stored-program computer](https://en.wikipedia.org/wiki/Stored-program_computer) 思想告诉我们: 在 memory 中，有如下两类:

1、instruction

2、data

在 `Function-and-data-model` 中，将基于此来构建起一个连接 software 和 hardware 的 uniform model。

---

## 二、Von Neumann architecture（冯·诺依曼架构）

### wikipedia [Von Neumann architecture](https://en.wikipedia.org/wiki/Von_Neumann_architecture)

See also: [Stored-program computer](https://en.wikipedia.org/wiki/Stored-program_computer) and [Universal Turing machine § Stored-program computer](https://en.wikipedia.org/wiki/Universal_Turing_machine#Stored-program_computer)

The **von Neumann architecture**—also known as the **von Neumann model** or **Princeton architecture**—is a [computer architecture](https://en.wikipedia.org/wiki/Computer_architecture) based on a 1945 description by the mathematician and physicist [John von Neumann](https://en.wikipedia.org/wiki/John_von_Neumann) and others in the *First Draft of a Report on the EDVAC*.[[1\]](https://en.wikipedia.org/wiki/Von_Neumann_architecture#cite_note-FirstDraftReport-1) That document describes a design architecture for an electronic [digital computer](https://en.wikipedia.org/wiki/Digital_computer) with these components:

- A [processing unit](https://en.wikipedia.org/wiki/Central_processing_unit) that contains an [arithmetic logic unit](https://en.wikipedia.org/wiki/Arithmetic_logic_unit) and [processor registers](https://en.wikipedia.org/wiki/Processor_register)
  
  > NOTE: 上面所说的 processing unit 指的就是 CPU

- A [control unit](https://en.wikipedia.org/wiki/Control_unit) that contains an [instruction register](https://en.wikipedia.org/wiki/Instruction_register) and [program counter](https://en.wikipedia.org/wiki/Program_counter)

- [Memory](https://en.wikipedia.org/wiki/Computer_memory) that stores [data](https://en.wikipedia.org/wiki/Data_(computing)) and [instructions](https://en.wikipedia.org/wiki/Instruction_set)
  
  > NOTE: 要充分理解上面这段话的含义，就需要阅读 See also: [Stored-program computer](https://en.wikipedia.org/wiki/Stored-program_computer) and [Universal Turing machine § Stored-program computer](https://en.wikipedia.org/wiki/Universal_Turing_machine#Stored-program_computer)

- External [mass storage](https://en.wikipedia.org/wiki/Mass_storage)

- [Input and output](https://en.wikipedia.org/wiki/Input_and_output) mechanisms[[1\]](https://en.wikipedia.org/wiki/Von_Neumann_architecture#cite_note-FirstDraftReport-1)[[2\]](https://en.wikipedia.org/wiki/Von_Neumann_architecture#cite_note-GanesanCh4-2)

The term "von Neumann architecture" has evolved to mean any [stored-program computer](https://en.wikipedia.org/wiki/Stored-program_computer) in which an [instruction fetch](https://en.wikipedia.org/wiki/Instruction_fetch) and a data operation cannot occur at the same time because they share a common [bus](https://en.wikipedia.org/wiki/Bus_(computing)). This is referred to as the [von Neumann bottleneck](https://en.wikipedia.org/wiki/Von_Neumann_architecture#Von_Neumann_bottleneck) and often limits the performance of the system.[[3\]](https://en.wikipedia.org/wiki/Von_Neumann_architecture#cite_note-3)

> NOTE: 术语“冯·诺依曼架构”已经发展为任何存储程序计算机，其中指令获取和数据操作不能同时发生，因为它们共享公共总线。 这被称为冯诺依曼瓶颈，并且经常限制系统的性能。

The design of a von Neumann architecture machine is simpler than a [Harvard architecture](https://en.wikipedia.org/wiki/Harvard_architecture) machine—which is also a **stored-program system** but has one dedicated set of address and data buses for reading and writing to memory, and another set of address and [data buses](https://en.wikipedia.org/wiki/Memory_bus) to fetch [instructions](https://en.wikipedia.org/wiki/Instruction_fetch).

A **stored-program digital computer** keeps both [program instructions](https://en.wikipedia.org/wiki/Computer_program) and data in [read-write](https://en.wikipedia.org/wiki/Read-write_memory), [random-access memory](https://en.wikipedia.org/wiki/Random-access_memory) (RAM). Stored-program computers were an advancement over the program-controlled computers of the 1940s, such as the [Colossus](https://en.wikipedia.org/wiki/Colossus_computer) and the [ENIAC](https://en.wikipedia.org/wiki/ENIAC). Those were programmed by setting switches and inserting patch cables to route data and control signals between various functional units. The vast majority of modern computers use the same memory for both data and program instructions. The von Neumann vs. Harvard distinction applies to the [cache](https://en.wikipedia.org/wiki/CPU_cache) architecture, not the main memory ([split cache architecture](https://en.wikipedia.org/wiki/Modified_Harvard_architecture#Split-cache_(or_Almost-von-Neumann)_architecture)).

![img](https://upload.wikimedia.org/wikipedia/commons/thumb/e/e5/Von_Neumann_Architecture.svg/220px-Von_Neumann_Architecture.svg.png)

Von Neumann architecture scheme

### History

The earliest computing machines had fixed programs. Some very simple computers still use this design, either for simplicity or training purposes. For example, a desk [calculator](https://en.wikipedia.org/wiki/Calculator) (in principle) is a fixed program computer. It can do basic [mathematics](https://en.wikipedia.org/wiki/Mathematics), but it cannot run a [word processor](https://en.wikipedia.org/wiki/Word_processor) or games. Changing the program of a fixed-program machine requires rewiring, restructuring, or redesigning the machine. The earliest computers were not so much "programmed" as "designed" for a particular task. "Reprogramming"—when possible at all—was a laborious process that started with [flowcharts](https://en.wikipedia.org/wiki/Flowchart) and paper notes, followed by detailed engineering designs, and then the often-arduous process of physically rewiring and rebuilding the machine. It could take three weeks to set up and debug a program on [ENIAC](https://en.wikipedia.org/wiki/ENIAC).[[4\]](https://en.wikipedia.org/wiki/Von_Neumann_architecture#cite_note-4)

> NOTE: 最早的计算机有固定的程序。一些非常简单的计算机仍然使用这种设计，无论是为了简单还是培训。例如，桌面计算器（原则上）是固定程序计算机。它可以做基础数学，但它不能运行文字处理器或游戏。更改固定程序机器的程序需要重新布线，重组或重新设计机器。最早的计算机并没有为特定任务“编程”为“设计”。 “重新编程” - 在可能的情况下 - 是一个艰苦的过程，从流程图和纸质笔记开始，然后是详细的工程设计，然后是经常艰苦的物理重新布线和重建机器的过程。在 ENIAC 上设置和调试程序可能需要三周时间。[4]

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

### 深入理解：冯·诺依曼「一切皆数据」（Everything is Data）

这是对前面 Capabilities（将指令视为数据）与 Function and data model 的进一步展开，也是理解「自修改代码」（见第四章）与「冯·诺依曼瓶颈」（见第三章）的概念基础。

#### 这句话的本质含义（存储程序原理核心）

**所有信息（指令、数字、文本、常量、代码）在内存里全是二进制比特，内存本身不区分"指令"和"普通数据"，统一编址、统一存储、统一总线访问**。

1. **编码层面：指令就是特殊的数据**
   程序指令（`ADD`/`LOAD`/跳转）、运算变量、字符串、数组，全部用相同二进制格式表示；一串 0/1，放在 PC 指向地址时就是指令，放在 Load 指令访问地址时就是数据。
2. **存储层面：统一地址空间**
   代码段、全局变量、栈、堆共处同一块线性内存地址，没有物理隔离。
   - 可以**自修改代码**：程序运行时，像读写普通变量一样改写指令内存（详见第四章 Self-modifying code）；
   - 可以动态加载程序、解释器、JIT 编译（Python/Java 虚拟机、浏览器 JS 引擎全部依赖这个特性）。
3. **硬件层面：一套总线共享**
   只有一组地址总线 + 一组数据总线，取指令、读写数据复用同一套通道，这也是**"冯·诺依曼瓶颈"的来源**（详见第三章）。

#### CPU 如何区分"指令"和"数据"？不靠内存，靠**执行时机**

内存单元没有标记位区分内容类型，CPU 靠流水线阶段判断身份：

1. **取指阶段（PC 寻址）**
   PC（程序计数器）输出地址 → MAR → 地址总线读内存，读出的二进制直接送入指令寄存器 IR，**强制当做指令译码执行**。
2. **执行阶段（Load/Store 寻址）**
   指令里的操作数地址送入 MAR，读出的二进制送入 MDR → 通用寄存器，**强制当做运算数据**。

同一个内存地址的同一串二进制：

- PC 访问 = 指令
- MOV/LOAD 访问 = 普通数据

完美印证：**存储介质眼里，万物都是数据**。

#### 对比哈佛结构：为什么它做不到"一切皆数据"

经典哈佛结构从硬件上割裂指令与数据，打破"统一数据视图"：

1. **两套物理独立存储器、两套独立地址空间**
   
   - 指令存储器：Flash/ROM，专属指令总线
   - 数据存储器：RAM，专属数据总线
   
   两者地址互不重叠，不能互相访问；无法用数据总线读取指令，也不能用指令总线读写变量。

2. **天然不支持自修改代码**
   指令区大多为只读 Flash，且没有通路让程序像操作数据一样改写指令存储区；嵌入式 MCU 无法动态加载、JIT。

3. **改良哈佛（现代 RISC-V/ARM Cortex-A）补充**
   片内 L1 ICache/DCache 分离（内部双总线），但**片外主存仍是统一地址空间**，软件层面依然满足"一切皆数据"，只是硬件缓存做并行加速，不属于纯哈佛限制。

#### "一切皆数据"带来的两大革命性价值

1. **通用计算机的根基**
   ENIAC 时代程序靠接线硬件固化；冯氏把程序变成可存储、可复制、可修改的二进制数据，**软件和硬件彻底分离**，才有编译器、操作系统、各类应用程序。
2. **强大的软件灵活性**
   - 解释型语言、虚拟机、动态链接、热更新全部依赖指令可被当作数据读写；
   - 操作系统统一管理内存，代码和数据可动态调度、交换到磁盘。

#### 极简总结

1. 冯·诺依曼「一切皆数据」：**指令本质是二进制数据，与变量共用同一内存、同一地址空间、同一总线，仅 CPU 流水线上下文区分用途**；
2. 经典哈佛结构因存储/总线物理隔离，不满足"一切皆数据"；
3. 带分离缓存的改良哈佛（桌面/服务器 RISC-V、ARM-A）**软件视角仍是冯氏统一内存，保留一切皆数据特性**。

---

## 三、Von Neumann bottleneck（冯·诺依曼瓶颈）

> NOTE: 记得大学时在学习**计算机组成原理**课程的时候，老师提出过重要的观点：“限制 CPU 速度的是从内存中读写数据”。意思是 CPU 的 ALU 的运算速度是非常快的，相比之下从内存中读取是比较缓慢的，所以 ALU 常常需要等待，这应该是当代 CPU 设计时需要考虑的一个矛盾所在，各种缓解这个矛盾的技术不断出现，比如:
> 
> 1. 在 Book-计算机组成原理-科学出版社的 5.1.3 CPU 中的主要寄存器章节中所描述的**数据缓冲寄存器（DR）**的作用：补偿 CPU 和内存、外围设备之间在操作速度上的差别。
> 
> 与 Von Neumann bottleneck 相关的一个问题是: IO-bound，关于此，参见工程 software-engineering 的 `Software-analysis\Performance\Bound` 章节。

### 3.1 冯·诺依曼架构回顾

1945 年，约翰·冯·诺依曼提出了影响至今的计算机体系结构——其核心特征是**存储程序（Stored Program）**：**指令和数据都存放在同一个内存中**。

```
        ┌─────────────────────────────┐
        │       CPU (处理器)           │
        │  ┌──────────┐  ┌──────────┐  │
        │  │ 控制单元  │  │ 运算单元  │  │
        │  │  (CU)    │  │  (ALU)   │  │
        │  └──────────┘  └──────────┘  │
        └──────────────┬──────────────┘
                       │
                       │  ⚠️ 单一总线（瓶颈所在）
                       │  指令 + 数据 共享此通道
                       │
        ┌──────────────┴──────────────┐
        │        Memory (内存)         │
        │     指令(Instructions)       │
        │     数据(Data)               │
        └─────────────────────────────┘
```

**关键特征**：

- 指令和数据存在**同一内存**
- CPU 与内存之间只有**一条共享总线**
- 指令的取指、译码、执行**顺序进行**

### 3.2 瓶颈的本质

#### 核心矛盾：处理器快，通道窄

```
CPU 计算速度：  ██████████████████████  极快（GHz 级）
                        ↕
内存传输速度：  ████                    慢得多（受总线带宽限制）
```

由于**指令和数据共享同一条总线**，且 CPU 一次只能通过这条总线传输一个数据/指令：

> **CPU 大量时间不是在"计算"，而是在"等待数据从内存搬过来"。**

这个"处理器与内存之间数据传输速率，限制了整体计算速度"的现象，就是**冯·诺依曼瓶颈**。这个术语由图灵奖得主 **John Backus** 在 1977 年的图灵奖演讲中正式提出。

#### 一个直观的比喻

```
CPU     = 一位手速极快的大厨（能瞬间做完菜）
内存     = 很远的仓库（存着所有食材）
总线     = 大厨与仓库之间唯一一条窄窄的传送带

结果：大厨大部分时间在【等食材】，而不是在【做菜】
      传送带（总线）的速度，决定了出菜速度
```

### 3.3 为什么会越来越严重（内存墙）

冯·诺依曼瓶颈随时间**不断恶化**，因为 CPU 和内存的发展速度严重不匹配：

```
性能增长（对数尺度）

  性能  │                              CPU 算力
        │                         ／
        │                    ／
        │               ／           ← 差距越来越大
        │          ／                  （剪刀差）
        │     ／
        │ ／ ─────────────────────── 内存带宽
        └────────────────────────────▶ 时间
```

- **CPU 性能**：长期遵循摩尔定律，快速增长
- **内存带宽/延迟**：增长缓慢得多

这个不断扩大的差距被称为 **"内存墙"（Memory Wall）**。

> **AI 时代的加剧**：GPU/NPU 的算力（尤其 TensorCore）暴涨，但 HBM 带宽跟不上——**"内存墙"在深度学习中比在传统计算中更为致命**。这正是"深度学习是访存密集型（Memory-Bound）"的根源。

### 3.4 缓解手段：硬件层面

工程师们发明了大量技术来"绕过"或"缓解"瓶颈（注意：是缓解，不是消除）：

#### 缓存（Cache）—— 最重要的缓解手段

在 CPU 和内存之间加入**高速小容量存储**，利用局部性原理。

```
        CPU
         │  ← 快
    ┌────┴────┐
    │ L1 Cache│  几 KB，最快
    ├─────────┤
    │ L2 Cache│  几百 KB
    ├─────────┤
    │ L3 Cache│  几 MB
    └────┬────┘
         │  ← 慢
      Memory     几 GB，最慢
```

**局部性原理（Locality）**：

- **时间局部性**：刚访问的数据很可能再次被访问
- **空间局部性**：访问某数据后，很可能访问其相邻数据

#### 哈佛架构 / 改进型哈佛架构

**分离指令和数据的存储与通道**——这直接针对瓶颈根源。

```
冯·诺依曼：指令+数据 共享一条总线
哈佛架构： 指令总线 ┃ 数据总线   分开（可同时传输）

现代 CPU：L1 Cache 常分为 I-Cache 和 D-Cache
         → "改进型哈佛架构"，在缓存层面分离
```

#### 其他硬件技术

| 技术                  | 原理                   |
| ------------------- | -------------------- |
| **多级流水线（Pipeline）** | 取指、译码、执行重叠进行         |
| **预取（Prefetch）**    | 提前把可能用到的数据搬进缓存       |
| **乱序执行**            | 在等待数据时，先执行其他不依赖的指令   |
| **多通道内存**           | 增加总线宽度/通道数，提高带宽      |
| **HBM（高带宽内存）**      | 3D 堆叠，大幅提高带宽（GPU 标配） |

### 3.5 缓解手段：软件/编译器层面

编译器和程序员也能通过优化**减少访存、提高缓存命中率**：

#### 提高数据局部性

```
- 循环分块（Loop Tiling）：让数据块能装进缓存后反复用
- 循环交换（Loop Interchange）：调整访问顺序，顺序访问内存
- 数据布局优化：让频繁一起访问的数据在内存中相邻
```

#### 减少不必要的访存

```
- 寄存器分配：把频繁使用的变量放寄存器，避免读写内存
- 公共子表达式消除：避免重复计算（也避免重复访存）
- 算子融合：中间结果不写回内存（见下节）
```

### 3.6 与 AI 编译器的深层关联

**这是本文的核心落脚点**——冯·诺依曼瓶颈是理解 AI 编译器所有"访存优化"的**底层第一性原理**。

#### "访存密集"的根源就是冯·诺依曼瓶颈

```
深度学习的性能瓶颈：访存 > 计算

为什么？
  因为算力（计算）增长远快于内存带宽增长
  → 就是"内存墙" / 冯·诺依曼瓶颈在 AI 场景的体现
```

#### 算子融合为什么有效？—— 直击瓶颈

回顾前面讲的**算子融合**，它之所以是图优化的"皇冠明珠"，本质就是**在对抗冯·诺依曼瓶颈**：

```
融合前：每个算子都要"从内存读→算→写回内存"
    Conv → [写内存] → BN → [写内存] → ReLU
    ↑ 每次读写都在挤那条窄窄的总线（瓶颈！）

融合后：中间结果留在寄存器/片上，不走总线
    ┌── Conv+BN+ReLU ──┐
    └─ 中间结果不落地 ─┘
    ↑ 大幅减少总线上的数据搬运 → 直接缓解瓶颈
```

> **深刻联系**：算子融合减少内存往返 = 减少走总线的次数 = 直接对抗冯·诺依曼瓶颈。

#### FlashAttention 的本质也是对抗瓶颈

```
标准 Attention：N×N 大矩阵写回 HBM（大量走总线 → 瓶颈爆炸）
FlashAttention：分块计算，中间结果留在 SRAM（片上）
    → 避免大矩阵走总线 → 完美对抗冯·诺依曼瓶颈
```

#### 一张图看懂：所有访存优化的共同目标

```
              冯·诺依曼瓶颈（底层根源）
                       │
          ┌────────────┼────────────┐
          ▼            ▼            ▼
       算子融合      内存复用      FlashAttention
          │            │            │
          └────────────┼────────────┘
                       ▼
            共同目标：减少数据在
            "内存↔计算单元"间的搬运
            = 减少走总线（瓶颈）的流量
```

### 3.7 超越冯·诺依曼的新架构

为从根本上突破瓶颈，业界探索**全新架构**：

| 新范式                              | 核心思想                           |
| -------------------------------- | ------------------------------ |
| **存内计算（In-Memory Computing）**    | 让计算发生在内存内部，数据不用搬到 CPU          |
| **近内存计算（Near-Memory Computing）** | 把计算单元放到内存旁边，缩短搬运距离             |
| **数据流架构（Dataflow）**              | 数据流动驱动计算，而非"取指-执行"（很多 AI 芯片采用） |
| **神经形态计算（Neuromorphic）**         | 模仿大脑，存算一体（如 Intel Loihi）       |

> **AI 芯片的趋势**：大量 NPU/AI 加速器采用**数据流架构**和**大片上存储（SRAM）**设计，本质都是为了绕开冯·诺依曼瓶颈——**把数据尽量留在离计算单元近的地方**。



### 3.8 总结

```
冯·诺依曼瓶颈 = 指令与数据共享单一总线
             → CPU 常在"等数据"，而非"在计算"
             → 内存带宽限制了整体性能

恶化趋势："内存墙"—— CPU 算力增长 >> 内存带宽增长

缓解手段：
  硬件层：Cache · 哈佛架构 · 预取 · 流水线 · HBM
  软件层：局部性优化 · 减少访存 · 算子融合

根本突破：存内计算 · 数据流架构 · 存算一体
```

#### 面试要点

| 问题              | 要点                          |
| --------------- | --------------------------- |
| **什么是冯·诺依曼瓶颈？** | 指令数据共享单一总线，内存传输速率限制整体性能     |
| **为什么越来越严重？**   | 内存墙——CPU 算力增长远快于内存带宽        |
| **缓存如何缓解？**     | 利用时间/空间局部性，减少访问慢速内存         |
| **哈佛架构如何解决？**   | 分离指令和数据总线，可并行传输             |
| **和深度学习什么关系？**  | 深度学习访存密集，瓶颈本质就是内存墙          |
| **算子融合为何有效？**   | 减少中间结果的内存往返 = 减少总线流量 = 对抗瓶颈 |
| **AI 芯片如何突破？**  | 数据流架构、大片上 SRAM、存算一体         |

> 📌 **一句话总结**：冯·诺依曼瓶颈是计算机体系结构最根本的性能限制——**"计算快、搬运慢"**。它是理解从 CPU 缓存设计，到 AI 编译器算子融合，再到 FlashAttention 和新型 AI 芯片架构的**共同底层逻辑**。
> 
> **对 AI 编译器从业者而言**：几乎所有"性能优化"最终都可以追溯到同一个目标——**减少数据在内存与计算单元之间的搬运，即对抗冯·诺依曼瓶颈**。理解了这一点，就抓住了 AI 编译器性能优化的"第一性原理"。

### 3.9 附：wikipedia 原文 [Von Neumann architecture # Design limitations # Von Neumann bottleneck](https://en.wikipedia.org/wiki/Von_Neumann_architecture#Von_Neumann_bottleneck)

The **shared bus** between the **program memory** and **data memory** leads to the *von Neumann bottleneck*, the limited [throughput](https://en.wikipedia.org/wiki/Throughput)(data transfer rate) between the [central processing unit](https://en.wikipedia.org/wiki/Central_processing_unit) (CPU) and memory compared to the amount of memory. Because the single bus can only access one of the two classes of memory at a time, throughput is lower than the rate at which the CPU can work. This seriously limits the effective processing speed when the CPU is required to perform minimal processing on large amounts of data. The CPU is continually [forced to wait](https://en.wikipedia.org/wiki/Wait_state) for needed data to move to or from memory. Since CPU speed and memory size have increased much faster than the throughput between them, the bottleneck has become more of a problem, a problem whose severity increases with every new generation of CPU.

The von Neumann bottleneck was described by [John Backus](https://en.wikipedia.org/wiki/John_Backus) in his 1977 ACM [Turing Award](https://en.wikipedia.org/wiki/Turing_Award) lecture. According to Backus:

> Surely there must be a less primitive way of making big changes in the store than by pushing vast numbers of [words](https://en.wikipedia.org/wiki/Word_(data_type)) back and forth through the von Neumann bottleneck. Not only is this tube a literal bottleneck for the data traffic of a problem, but, more importantly, it is an intellectual bottleneck that has kept us tied to word-at-a-time thinking instead of encouraging us to think in terms of the larger conceptual units of the task at hand. Thus programming is basically planning and detailing the enormous traffic of words through the von Neumann bottleneck, and much of that traffic concerns not significant data itself, but where to find it.[[26\]](https://en.wikipedia.org/wiki/Von_Neumann_architecture#cite_note-backus-26)[[27\]](https://en.wikipedia.org/wiki/Von_Neumann_architecture#cite_note-27)

#### Mitigations（wikipedia 列举的缓解手段）

There are several known methods for mitigating the Von Neumann performance bottleneck. For example, the following all can improve performance[*why?*]:

- Providing a [cache](https://en.wikipedia.org/wiki/CPU_cache) between the CPU and the [main memory](https://en.wikipedia.org/wiki/Main_memory)
- providing separate caches or separate access paths for data and instructions (the so-called [Modified Harvard architecture](https://en.wikipedia.org/wiki/Modified_Harvard_architecture))
- using [branch predictor](https://en.wikipedia.org/wiki/Branch_predictor) algorithms and logic
- providing a limited CPU stack or other on-chip [scratchpad memory](https://en.wikipedia.org/wiki/Scratchpad_memory) to reduce memory access
- Implementing the CPU and the [memory hierarchy](https://en.wikipedia.org/wiki/Memory_hierarchy) as a [system on chip](https://en.wikipedia.org/wiki/System_on_a_chip), providing greater [locality of reference](https://en.wikipedia.org/wiki/Locality_of_reference) and thus reducing latency and increasing throughput between [processor registers](https://en.wikipedia.org/wiki/Processor_register) and [main memory](https://en.wikipedia.org/wiki/Main_memory)

The problem can also be sidestepped somewhat by using [parallel computing](https://en.wikipedia.org/wiki/Parallel_computing), using for example the [non-uniform memory access](https://en.wikipedia.org/wiki/Non-uniform_memory_access) (NUMA) architecture—this approach is commonly employed by supercomputers. It is less clear whether the *intellectual bottleneck* that Backus criticized has changed much since 1977. Backus's proposed solution has not had a major influence.[*citation needed*] Modern [functional programming](https://en.wikipedia.org/wiki/Functional_programming) and [object-oriented programming](https://en.wikipedia.org/wiki/Object-oriented_programming) are much less geared towards "pushing vast numbers of words back and forth" than earlier languages like [FORTRAN](https://en.wikipedia.org/wiki/FORTRAN) were, but internally, that is still what computers spend much of their time doing, even highly parallel supercomputers.

As of 1996, a database benchmark study found that three out of four CPU cycles were spent waiting for memory. Researchers expect that increasing the number of simultaneous instruction streams with [multithreading](https://en.wikipedia.org/wiki/Multithreading_(computer_architecture)) or single-chip [multiprocessing](https://en.wikipedia.org/wiki/Multiprocessing) will make this bottleneck even worse.[[28\]](https://en.wikipedia.org/wiki/Von_Neumann_architecture#cite_note-28) In the context of [multi-core processors](https://en.wikipedia.org/wiki/Multi-core_processor), additional [overhead](https://en.wikipedia.org/wiki/Overhead_(computing)) is required to maintain [cache coherence](https://en.wikipedia.org/wiki/Cache_coherence) between processors and threads.

---

## 四、Self-modifying code

Aside from the von Neumann bottleneck, program modifications can be quite harmful, either by accident or design. In some simple stored-program computer designs, a malfunctioning program can damage itself, other programs, or the [operating system](https://en.wikipedia.org/wiki/Operating_system), possibly leading to a computer [crash](https://en.wikipedia.org/wiki/Crash_(computing)). [Memory protection](https://en.wikipedia.org/wiki/Memory_protection) and other forms of [access control](https://en.wikipedia.org/wiki/Access_control) can usually protect against both accidental and malicious program modification.
