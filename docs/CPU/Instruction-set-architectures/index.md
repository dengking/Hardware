# Instruction set architecture

## wikipedia [Instruction set architecture](https://en.wikipedia.org/wiki/Instruction_set_architecture)	

An **instruction set architecture** (**ISA**) is an abstract model of a [computer](https://en.wikipedia.org/wiki/Computer). It is also referred to as **architecture** or **computer architecture**. A realization of an ISA is called an *implementation*. An ISA permits multiple implementations that may vary in [performance](https://en.wikipedia.org/wiki/Computer_performance), physical size, and monetary cost (among other things); because the ISA serves as the [interface](https://en.wikipedia.org/wiki/Interface_(computing)) between [software](https://en.wikipedia.org/wiki/Software) and [hardware](https://en.wikipedia.org/wiki/Computer_hardware). Software that has been written for an ISA can run on different implementations of the same ISA. This has enabled [binary compatibility](https://en.wikipedia.org/wiki/Binary_compatibility) between different generations of computers to be easily achieved, and the development of **computer families**. Both of these developments have helped to lower the cost of computers and to increase their applicability. For these reasons, the ISA is one of the most important abstractions in [computing](https://en.wikipedia.org/wiki/Computing) today.

> NOTE: 这段话在《计算机组成原理-4.1-指令系统的发展和性能要求》中也有提及。ISA和**系列计算机**的概念密切相关。

An ISA defines everything a [machine language](https://en.wikipedia.org/wiki/Machine_language) [programmer](https://en.wikipedia.org/wiki/Programmer) needs to know in order to program a computer. What an ISA defines differs between ISAs; in general, ISAs define the supported [data types](https://en.wikipedia.org/wiki/Data_type), what state there is (such as the [main memory](https://en.wikipedia.org/wiki/Main_memory)and [registers](https://en.wikipedia.org/wiki/Processor_register)) and their semantics (such as the [memory consistency](https://en.wikipedia.org/wiki/Memory_consistency) and [addressing modes](https://en.wikipedia.org/wiki/Addressing_mode)), the *instruction set* (the set of [machine instructions](https://en.wikipedia.org/wiki/Machine_instruction) that comprises a computer's machine language), and the [input/output](https://en.wikipedia.org/wiki/Input/output) model.

An ISA specifies the behavior of [machine code](https://en.wikipedia.org/wiki/Machine_code) running on implementations of that ISA in a fashion that does not depend on the characteristics of that implementation, providing [binary compatibility](https://en.wikipedia.org/wiki/Binary_compatibility) between implementations. This enables multiple implementations of an ISA that differ in [performance](https://en.wikipedia.org/wiki/Computer_performance), physical size, and monetary cost (among other things), but that are capable of running the same machine code, so that a lower-performance, lower-cost machine can be replaced with a higher-cost, higher-performance machine without having to replace software. It also enables the evolution of the [microarchitectures](https://en.wikipedia.org/wiki/Microarchitecture) of the implementations of that ISA, so that a newer, higher-performance implementation of an ISA can run software that runs on previous generations of implementations.

> NOTE: 上面这段话所描述的其实是分离抽象与实现带来的好处。

If an [operating system](https://en.wikipedia.org/wiki/Operating_system) maintains a standard and compatible [application binary interface](https://en.wikipedia.org/wiki/Application_binary_interface) (ABI) for a particular ISA, machine code for that ISA and operating system will run on future implementations of that ISA and newer versions of that operating system. However, if an ISA supports running multiple operating systems, it does not guarantee that machine code for one operating system will run on another operating system, unless the first operating system supports running machine code built for the other operating system.

An ISA can be extended by adding instructions or other capabilities, or adding support for larger addresses and data values; an implementation of the extended ISA will still be able to execute machine code for versions of the ISA without those extensions. Machine code using those extensions will only run on implementations that support those extensions.

The binary compatibility that they provide make ISAs one of the most fundamental abstractions in [computing](https://en.wikipedia.org/wiki/Computing).

### Example

下面罗列了一些比较常见的ISA:

- [x86](https://en.wikipedia.org/wiki/X86)

- [ARM](https://en.wikipedia.org/wiki/ARM_architecture)

- [MIPS](https://en.wikipedia.org/wiki/MIPS_architecture)

- [Power ISA](https://en.wikipedia.org/wiki/Power_ISA)

- [SPARC](https://en.wikipedia.org/wiki/SPARC)

- [Itanium](https://en.wikipedia.org/wiki/IA-64)

  

### [Classification of ISAs](https://en.wikipedia.org/wiki/Instruction_set_architecture)

[complex instruction set computer](https://en.wikipedia.org/wiki/Complex_instruction_set_computer) (CISC) 

[reduced instruction set computer](https://en.wikipedia.org/wiki/Reduced_instruction_set_computer) (RISC)



### Instructions

[Machine language](https://en.wikipedia.org/wiki/Machine_code) is built up from discrete *statements* or *instructions*. On the processing architecture, a given instruction may specify:

- particular [registers](https://en.wikipedia.org/wiki/Processor_register) (for arithmetic, addressing, or control functions)
- particular memory locations (or offsets to them)
- particular [addressing modes](https://en.wikipedia.org/wiki/Addressing_mode) (used to interpret the operands)

More complex operations are built up by combining these simple instructions, which are executed sequentially, or as otherwise directed by [control flow](https://en.wikipedia.org/wiki/Control_flow) instructions.



### [Instruction types](https://en.wikipedia.org/wiki/Instruction_set_architecture#Instruction_types)



#### [Data handling and memory operations](https://en.wikipedia.org/wiki/Instruction_set_architecture#Data_handling_and_memory_operations)



#### [Arithmetic and logic](https://en.wikipedia.org/wiki/Arithmetic_logic_unit) operations

> NOTE: 这是最最基本的指令

#### [Control flow](https://en.wikipedia.org/wiki/Control_flow) operations

> NOTE: 控制流指令

#### [Coprocessor](https://en.wikipedia.org/wiki/Coprocessor) instructions



## wikipedia [Machine code](https://en.wikipedia.org/wiki/Machine_code)

> NOTE: 机器码，都是01。

Machine code is a strictly numerical language which is intended to run as fast as possible