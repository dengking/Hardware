# Load–store architecture

load 对应的是read data from memory；

store 对应的是write data to memory；


## wikipedia [Load–store architecture](https://en.wikipedia.org/wiki/Load%E2%80%93store_architecture)

In [computer engineering](https://en.wikipedia.org/wiki/Computer_engineering), a **load–store architecture** is an [instruction set architecture](https://en.wikipedia.org/wiki/Instruction_set_architecture) that divides instructions into two categories: [memory](https://en.wikipedia.org/wiki/Memory_(computing)) access ([load and store](https://en.wikipedia.org/wiki/Load_and_store) between memory and [registers](https://en.wikipedia.org/wiki/Processor_register)), and [ALU](https://en.wikipedia.org/wiki/Arithmetic_logic_unit) operations (which only occur between registers).[[1\]](https://en.wikipedia.org/wiki/Load–store_architecture#cite_note-Flynn-1):9-12

>NOTE: 显然，在 **load–store architecture** 中，所有的operand必须要先load到ALU中，ALU在执行指令的过程中，是不会access memory的；在ALU运算完成后，就将计算的结果store到memory中；

[RISC](https://en.wikipedia.org/wiki/Reduced_instruction_set_computing) instruction set architectures such as [PowerPC](https://en.wikipedia.org/wiki/PowerPC), [SPARC](https://en.wikipedia.org/wiki/SPARC), [RISC-V](https://en.wikipedia.org/wiki/RISC-V), [ARM](https://en.wikipedia.org/wiki/ARM_architecture), and [MIPS](https://en.wikipedia.org/wiki/MIPS_architecture) are **load–store architectures**.[[1\]](https://en.wikipedia.org/wiki/Load–store_architecture#cite_note-Flynn-1):9–12

For instance, in a **load–store approach** both operands and destination for an **ADD operation** must be in **registers**. This differs from a [register–memory architecture](https://en.wikipedia.org/wiki/Register_memory_architecture) (for example, a [CISC](https://en.wikipedia.org/wiki/Complex_instruction_set_computing) instruction set architecture such as [x86](https://en.wikipedia.org/wiki/X86)) in which one of the operands for the ADD operation may be in memory, while the other is in a register.[[1\]](https://en.wikipedia.org/wiki/Load–store_architecture#cite_note-Flynn-1):9–12

The earliest example of a load–store architecture was the [CDC 6600](https://en.wikipedia.org/wiki/CDC_6600).[[1\]](https://en.wikipedia.org/wiki/Load–store_architecture#cite_note-Flynn-1):54–56 Almost all [vector processors](https://en.wikipedia.org/wiki/Vector_processor) (including many [GPUs](https://en.wikipedia.org/wiki/GPUs)[[2\]](https://en.wikipedia.org/wiki/Load–store_architecture#cite_note-2)) use the load–store approach.[[3\]](https://en.wikipedia.org/wiki/Load–store_architecture#cite_note-3)



### See also

- [Load–store unit](https://en.wikipedia.org/wiki/Load–store_unit)
- [Register memory architecture](https://en.wikipedia.org/wiki/Register_memory_architecture)



## chortle [Load and Store](https://chortle.ccsu.edu/AssemblyTutorial/Chapter-15/ass15_2.html)

The operands for all arithmetic and logic operations are contained in registers. To operate on data in main memory, the data is first copied into registers. A **load** operation copies data from main memory into a register. A **store** operation copies data from a register into main memory .

When a word (4 bytes) is loaded or stored the memory address must be a multiple of four. This is called an alignment restriction. Addresses that are a multiple of four are called **word aligned**. This restriction makes the hardware simpler and faster.

The `lw` instruction loads a word into a register from memory. The `sw` instruction stores a word from a register into memory. Each instruction specifies a register and a memory address (details in a few pages).

## reading list



https://azeria-labs.com/memory-instructions-load-and-store-part-4/

