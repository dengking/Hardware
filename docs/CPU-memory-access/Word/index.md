# Word 

CPU memory access的unit。

## wikipedia [Word (computer architecture)](https://en.wikipedia.org/wiki/Word_(computer_architecture))

In [computing](https://en.wikipedia.org/wiki/Computing), a **word** is the natural unit of data used by a particular [processor](https://en.wikipedia.org/wiki/Central_processing_unit) design. A word is a fixed-sized [piece of data](https://en.wikipedia.org/wiki/Data_(computing)) handled as a unit by the [instruction set](https://en.wikipedia.org/wiki/Instruction_set) or the hardware of the processor. The number of [bits](https://en.wikipedia.org/wiki/Bit) in a word (the *word size*, *word width*, or *word length*) is an important characteristic of any specific processor design or [computer architecture](https://en.wikipedia.org/wiki/Computer_architecture).

> NOTE: 按照Book-计算机组成原理-科学出版社的1.2.4 计算机的性能指标 中给出的定义比较容易理解：
>
> 指处理机**运算器**（ALU）中一次能够完成二进制数运算的**位数**

The size of a word is reflected in many aspects of a computer's structure and operation; the majority of the [registers](https://en.wikipedia.org/wiki/Processor_register) in a processor are usually word sized and the largest piece of data that can be transferred to and from the [working memory](https://en.wikipedia.org/wiki/Computer_memory) in a single operation is a word in many (not all) architectures. The largest possible [address](https://en.wikipedia.org/wiki/Memory_address) size, used to designate a location in memory, is typically a hardware word (here, "hardware word" means the full-sized natural word of the processor, as opposed to any other definition used).

### Uses of words

Depending on how a computer is organized, **word-size units** may be used for:

### **Fixed point numbers**

Holders for [fixed point](https://en.wikipedia.org/wiki/Fixed-point_arithmetic), usually [integer](https://en.wikipedia.org/wiki/Integer_(computer_science)), numerical values may be available in one or in several different sizes, but **one of the sizes available will almost always be the word**（programmer可以据此来确定word size）. The other sizes, if any, are likely to be multiples or fractions of the word size. The smaller sizes are normally used only for efficient use of memory; when loaded into the processor, their values usually go into a larger, word sized holder.

### **Floating point numbers**

Holders for [floating point](https://en.wikipedia.org/wiki/Floating_point) numerical values are typically either a word or a multiple of a word.

### **Addresses**

Holders for memory addresses must be of a size capable of expressing the needed range of values but not be excessively large, so often the size used is the word though it can also be a multiple or fraction of the word size.

> NOTE: 
>
> 1、即指针类型的长度(pointer)

### **Registers**

[Processor registers](https://en.wikipedia.org/wiki/Processor_register) are designed with a size appropriate for the type of data they hold, e.g. integers, floating-point numbers, or addresses. Many computer architectures use [general-purpose registers](https://en.wikipedia.org/wiki/General_purpose_register) that are capable of storing data in multiple representations. These registers must be sized to hold the largest of the available types. Historically, this determined the word size of the architecture.

> NOTE: 
>
> 1、 [general-purpose registers](https://en.wikipedia.org/wiki/General_purpose_register) 的长度和Word的长度相等

### **Memory–processor transfer**

When the processor reads from the memory subsystem into a register or writes a register's value to memory, the amount of data transferred is often a **word**. Historically, this amount of bits which could be transferred in one cycle was also called a *catena* in some environments. In simple memory subsystems, the word is transferred over the memory [data bus](https://en.wikipedia.org/wiki/Bus_(computing)), which typically has a width of a word or half-word. In memory subsystems that use [caches](https://en.wikipedia.org/wiki/CPU_cache), the word-sized transfer is the one between the processor and the first level of cache; at lower levels of the [memory hierarchy](https://en.wikipedia.org/wiki/Memory_hierarchy) larger transfers (which are a multiple of the word size) are normally used.

> NOTE: 这个非常重要，后面的章节会对此进行详细讨论

### **Unit of address resolution**

In a given architecture, successive address values designate successive units of memory; this unit is the unit of address resolution. In most computers, the unit is either a character (e.g. a byte) or a word. (A few computers have used bit resolution.) If the unit is a word, then a larger amount of memory can be accessed using an address of a given size at the cost of added complexity to access individual characters. On the other hand, if the unit is a byte, then individual characters can be addressed (i.e. selected during the memory operation).

> NOTE: 这个和前面的**Memory–processor transfer**是紧密相关的，后面章节会进行详细讨论。

### **Instructions**

[Machine instructions](https://en.wikipedia.org/wiki/Machine_instruction) are normally the size of the architecture's word, such as in [RISC architectures](https://en.wikipedia.org/wiki/RISC_architectures), or a multiple of the "char" size that is a fraction of it. This is a natural choice since instructions and data usually share the same memory subsystem. In [Harvard architectures](https://en.wikipedia.org/wiki/Harvard_architecture) the word sizes of instructions and data need not be related, as instructions and data are stored in different memories; for example, [the processor in the 1ESS electronic telephone switch](https://en.wikipedia.org/wiki/1ESS_switch#1ESS_computer) had 37-bit instructions and 23-bit data words.




