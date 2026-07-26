# Load–store architecture

load 对应的是read data from memory to register；

store 对应的是write data from register to memory；


## wikipedia [Load–store architecture](https://en.wikipedia.org/wiki/Load%E2%80%93store_architecture)

In [computer engineering](https://en.wikipedia.org/wiki/Computer_engineering), a **load–store architecture** is an [instruction set architecture](https://en.wikipedia.org/wiki/Instruction_set_architecture) that divides instructions into two categories: [memory](https://en.wikipedia.org/wiki/Memory_(computing)) access ([load and store](https://en.wikipedia.org/wiki/Load_and_store) between memory and [registers](https://en.wikipedia.org/wiki/Processor_register)), and [ALU](https://en.wikipedia.org/wiki/Arithmetic_logic_unit) operations (which only occur between registers).[[1\]](https://en.wikipedia.org/wiki/Load–store_architecture#cite_note-Flynn-1):9-12

>NOTE: 显然，在 **load–store architecture** 中，所有的operand必须要先load到ALU中，ALU在执行指令的过程中，是不会access memory的；在ALU运算完成后，就将计算的结果store到memory中；

[RISC](https://en.wikipedia.org/wiki/Reduced_instruction_set_computing) instruction set architectures such as [PowerPC](https://en.wikipedia.org/wiki/PowerPC), [SPARC](https://en.wikipedia.org/wiki/SPARC), [RISC-V](https://en.wikipedia.org/wiki/RISC-V), [ARM](https://en.wikipedia.org/wiki/ARM_architecture), and [MIPS](https://en.wikipedia.org/wiki/MIPS_architecture) are **load–store architectures**.[[1\]](https://en.wikipedia.org/wiki/Load–store_architecture#cite_note-Flynn-1):9–12

For instance, in a **load–store approach** both operands and destination for an **ADD operation** must be in **registers**. This differs from a [register–memory architecture](https://en.wikipedia.org/wiki/Register_memory_architecture) (for example, a [CISC](https://en.wikipedia.org/wiki/Complex_instruction_set_computing) instruction set architecture such as [x86](https://en.wikipedia.org/wiki/X86)) in which one of the operands for the ADD operation may be in memory, while the other is in a register.[[1\]](https://en.wikipedia.org/wiki/Load–store_architecture#cite_note-Flynn-1):9–12

The earliest example of a load–store architecture was the [CDC 6600](https://en.wikipedia.org/wiki/CDC_6600).[[1\]](https://en.wikipedia.org/wiki/Load–store_architecture#cite_note-Flynn-1):54–56 Almost all [vector processors](https://en.wikipedia.org/wiki/Vector_processor) (including many [GPUs](https://en.wikipedia.org/wiki/GPUs)[[2\]](https://en.wikipedia.org/wiki/Load–store_architecture#cite_note-2)) use the load–store approach.[[3\]](https://en.wikipedia.org/wiki/Load–store_architecture#cite_note-3)



## gpt-4-vision Load–store architecture 

**Load-store architecture** is a design approach used in computer processors where most instructions cannot directly manipulate memory, except for specific instructions designed to load data from memory into processor registers and store data from registers back into memory. This architecture is also known as a register-register or load/store architecture.

In a load-store architecture, the CPU operates primarily on data that is held in its registers. The majority of the arithmetic and logical operations are performed between these registers. If data needs to be processed, it must first be loaded from memory into a register. Similarly, after processing, if the data needs to be saved, it must be stored back into memory from a register.

Key characteristics of load-store architecture include:

1. **Separation of Memory and Computational Instructions**: The architecture clearly distinguishes between instructions that move data between memory and registers (load and store) and instructions that perform operations on data in the registers.

2. **Limited Memory Access**: Only load and store instructions can access memory. All other instructions operate on register contents.

3. **Large Register Set**: To minimize the need for frequent memory access, load-store architectures often provide a larger set of registers to hold intermediate data and operands.

4. **Simpler Instruction Set**: The separation of memory access from computational instructions often leads to a simpler and more orthogonal instruction set, which can make the processor design more efficient and easier to optimize.

5. **Efficient Pipelining**: With a clear separation of memory access and computation, it's easier to design pipelines and schedule instructions for better CPU utilization and performance.

Examples of load-store architectures include the RISC (Reduced Instruction Set Computer) architectures like ARM, MIPS, and RISC-V, as well as some DSP (Digital Signal Processor) architectures. These are contrasted with CISC (Complex Instruction Set Computer) architectures like x86, where instructions can often operate directly on memory.

The load-store model is a fundamental part of the RISC philosophy, which aims to simplify the instruction set of the processor to enable faster instruction execution and easier optimization by the compiler. This simplicity often leads to more consistent and predictable performance, which is one reason why RISC architectures are prevalent in embedded systems and mobile devices.



## reading list

chortle [Load and Store](https://chortle.ccsu.edu/AssemblyTutorial/Chapter-15/ass15_2.html)

https://azeria-labs.com/memory-instructions-load-and-store-part-4/

