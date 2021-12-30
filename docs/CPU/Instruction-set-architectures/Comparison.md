# [Comparison of instruction set architectures](https://en.wikipedia.org/wiki/Comparison_of_instruction_set_architectures)	

An **instruction set architecture** (**ISA**) is an abstract model of a [computer](https://en.wikipedia.org/wiki/Computer). It is also referred to as **architecture** or **computer architecture**. A realization of an ISA is called an *implementation*. An ISA permits multiple implementations that may vary in [performance](https://en.wikipedia.org/wiki/Computer_performance), physical size, and monetary cost (among other things); because the ISA serves as the [interface](https://en.wikipedia.org/wiki/Interface_(computing)) between [software](https://en.wikipedia.org/wiki/Software) and [hardware](https://en.wikipedia.org/wiki/Computer_hardware). Software that has been written for an ISA can run on different implementations of the same ISA. This has enabled [binary compatibility](https://en.wikipedia.org/wiki/Binary_compatibility) between different generations of computers to be easily achieved, and the development of computer families. Both of these developments have helped to lower the cost of computers and to increase their applicability. For these reasons, the ISA is one of the most important abstractions in [computing](https://en.wikipedia.org/wiki/Computing) today.

An ISA defines everything a [machine language](https://en.wikipedia.org/wiki/Machine_language) [programmer](https://en.wikipedia.org/wiki/Programmer) needs to know in order to program a computer. What an ISA defines differs between ISAs; in general, ISAs define the supported [data types](https://en.wikipedia.org/wiki/Data_type), what state there is (such as the [main memory](https://en.wikipedia.org/wiki/Main_memory) and [registers](https://en.wikipedia.org/wiki/Processor_register)) and their semantics (such as the [memory consistency](https://en.wikipedia.org/wiki/Memory_consistency) and [addressing modes](https://en.wikipedia.org/wiki/Addressing_mode)), the *instruction set* (the set of [machine instructions](https://en.wikipedia.org/wiki/Machine_instruction) that comprises a computer's machine language), and the [input/output](https://en.wikipedia.org/wiki/Input/output) model.



## Base

In the early decades of computing, there were computers that used [binary](https://en.wikipedia.org/wiki/Binary_number), [decimal](https://en.wikipedia.org/wiki/Decimal_computer)[[1\]](https://en.wikipedia.org/wiki/Comparison_of_instruction_set_architectures#cite_note-1) and even [ternary](https://en.wikipedia.org/wiki/Ternary_computer).[[2\]](https://en.wikipedia.org/wiki/Comparison_of_instruction_set_architectures#cite_note-2)[[3\]](https://en.wikipedia.org/wiki/Comparison_of_instruction_set_architectures#cite_note-3) Contemporary computers are almost exclusively binary.

### Bits

[Computer architectures](https://en.wikipedia.org/wiki/Computer_architecture) are often described as *n*-[bit](https://en.wikipedia.org/wiki/Bit) architectures. Today *n* is often 8, 16, 32, or 64, but other sizes have been used. This is actually a strong simplification. A computer architecture often has a few more or less "natural" datasizes in the [instruction set](https://en.wikipedia.org/wiki/Instruction_set), but the hardware implementation of these may be very different. Many architectures have instructions operating on half and/or twice the size of respective processors' major internal datapaths. Examples of this are the [8080](https://en.wikipedia.org/wiki/8080), [Z80](https://en.wikipedia.org/wiki/Z80), [MC68000](https://en.wikipedia.org/wiki/MC68000) as well as many others. On this type of implementations, a twice as wide operation typically also takes around twice as many clock cycles (which is not the case on high performance implementations). On the 68000, for instance, this means 8 instead of 4 clock ticks, and this particular chip may be described as a [32-bit](https://en.wikipedia.org/wiki/32-bit)architecture with a [16-bit](https://en.wikipedia.org/wiki/16-bit) implementation. The external databus width is often not useful to determine the width of the architecture; the [NS32008, NS32016 and NS32032](https://en.wikipedia.org/wiki/NS320xx) were basically the same 32-bit chip with different external data buses. The NS32764 had a [64-bit](https://en.wikipedia.org/wiki/64-bit_computing) bus, but used 32-bit registers.

The width of addresses may or may not be different from the width of data. Early 32-bit microprocessors often had a 24-bit address, as did the [System/360](https://en.wikipedia.org/wiki/System/360) processors.

### Operands

Main article: [instruction set § Number of operands](https://en.wikipedia.org/wiki/Instruction_set#Number_of_operands)

The number of operands is one of the factors that may give an indication about the performance of the instruction set. A three-operand architecture will allow

```
A := B + C
```

to be computed in one instruction.

A two-operand architecture will allow

```
A := A + B
```

to be computed in one instruction, so two instructions will need to be executed to simulate a single three-operand instruction

```
A := B
A := A + C
```

### Endianness

An architecture may use "big" or "little" endianness, or both, or be configurable to use either. Little endian processors order [bytes](https://en.wikipedia.org/wiki/Byte) in memory with the least significant byte of a multi-byte value in the lowest-numbered memory location. Big endian architectures instead order them with the most significant byte at the lowest-numbered address. The x86 architecture as well as several [8-bit](https://en.wikipedia.org/wiki/8-bit) architectures are little endian. Most [RISC](https://en.wikipedia.org/wiki/RISC) architectures (SPARC, Power, PowerPC, MIPS) were originally big endian (ARM was little endian), but many (including ARM) are now configurable.

Endianness *only* applies to processors that allow individual addressing of units of data (such as [bytes](https://en.wikipedia.org/wiki/Bytes)) that are *smaller* than the basic addressable machine word.

## Instruction sets

Usually the number of registers is a power of two, e.g. 8, 16, 32. In some cases a hardwired-to-zero pseudo-register is included, as "part" of [register files](https://en.wikipedia.org/wiki/Register_file) of architectures, mostly to simplify indexing modes. This table only counts the integer "registers" usable by general instructions at any moment. Architectures always include special-purpose registers such as the program pointer (PC). Those are not counted unless mentioned. Note that some architectures, such as SPARC, have [register window](https://en.wikipedia.org/wiki/Register_window); for those architectures, the count below indicates how many registers are available within a register window. Also, non-architected registers for [register renaming](https://en.wikipedia.org/wiki/Register_renaming#Architectural_vs_physical_registers) are not counted.

Note, a common type of architecture, "load-store", is a synonym for "Register Register" below, meaning no instructions access memory except special – load to register(s) – and store from register(s) – with the possible exceptions of atomic memory operations for locking.

The table below compares basic information about instruction sets to be implemented in the CPU architectures:



> NOTE: 这些不同的instruction set往往用于不同的领域，如[arm往往在手机、智能移动设备中](https://zhidao.baidu.com/question/153638698.html)。

