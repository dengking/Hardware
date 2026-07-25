# Registers

本章描述register，它是理解很多内容的基础；关于具体architecture中的register，参见:

| architecture | 章节          |
| ------------ | ----------- |
| ARM          | `CPU\ARM`   |
| Intel        | `CPU\Intel` |

除了本工程，在下面的工程中也涉及了register：

| 工程                     | 章节                                                                                                              |
| ---------------------- | --------------------------------------------------------------------------------------------------------------- |
| 工程programming-language | `C-family-language\C-and-C++\From-source-code-to-exec\ABI\Call-convention`章节，涉及到Stack-register                  |
| 工程Linux-OS             | `Shell-and-tools\Tools\Debug\GDB\Debugging-with-gdb\10-Examining-Data\10.13-Registers.md`章节，其中描述了如何查看register的值 |

## Architecture’s canonical mnemonics for registers.

不同的Architecture对于register往往采用不同的canonical mnemonics（助记符），非常典型的就是Intel和ARM，关于此，详见描述它们的章节。

## Program counter

这个寄存器非常重要，它告诉CPU去执行哪一条指令。所有对 [control flow](https://en.wikipedia.org/wiki/Control_flow) 的操作的指令最终都是通过操作这个寄存器的值来实现的。[Program counter](https://en.wikipedia.org/wiki/Program_counter)所指向的肯定是code area，[Program counter](https://en.wikipedia.org/wiki/Program_counter)相当于next pointer，默认情况下它是自加1的，除非通过[JMP (x86 instruction)](https://en.wikipedia.org/wiki/JMP_(x86_instruction))等指令来显示更改它的值。

### wikipedia [Program counter](https://en.wikipedia.org/wiki/Program_counter)



## Stack register

### wikipedia [Stack register](https://en.wikipedia.org/wiki/Stack_register)

A **stack register** is a computer central [processor register](https://en.wikipedia.org/wiki/Processor_register) whose purpose is to keep track of a [call stack](https://en.wikipedia.org/wiki/Call_stack). On an [accumulator-based architecture](https://en.wikipedia.org/wiki/Accumulator-based_architecture) machine, this may be a dedicated register such as SP on an [Intel x86](https://en.wikipedia.org/wiki/Intel_x86) machine. 

#### Stack registers in x86

In [8086](https://en.wikipedia.org/wiki/8086), the main stack register is called stack pointer - SP. The stack segment register (SS) is usually used to store information about the [memory segment](https://en.wikipedia.org/wiki/Memory_segment) that stores the [call stack](https://en.wikipedia.org/wiki/Call_stack) of currently executed program. SP points to **current stack top**. By default, the stack grows downward in memory, so newer values are placed at lower memory addresses. To push a value to the stack, the `PUSH` instruction is used. To pop a value from the stack, the `POP` instruction is used.

> NOTE: 对于 [8086](https://en.wikipedia.org/wiki/8086) ， SP和SS需要一起才能够工作。

**Example**: Assuming that SS = 1000h and SP = 0xF820. This means that current stack top is the physical address 0x1F820 (this is due to [memory segmentation in 8086](https://en.wikipedia.org/wiki/Intel_8086#Segmentation)). The next two machine instructions of the program are:

```assembly
PUSH AX
PUSH BX
```

- These first instruction shall push the value stored in AX (16-bit register) to the stack. This is done by subtracting a value of 2 (2 bytes) from SP.
- The new value of SP becomes 0xF81E. The CPU then copies the value of AX to the memory word whose physical address is 0x1F81E.
- When "PUSH BX" is executed, SP is set to 0xF81C and BX is copied to 0x1F81C. 

> NOTE: 上面描述的过程就相当于函数在执行的时候，给自动变量分配内存空间。

This illustrates how PUSH works. Usually, the running program pushes registers to the stack to make use of the registers for other purposes, like to call a routine that may change the current values of registers. To restore the values stored at the stack, the program shall contain machine instructions like this:

```
POP BX
POP AX
```

- `POP BX` copies the word at 0x1F81C (which is the old value of BX) to BX, then increases SP by 2. SP now is 0xF81E.
- `POP AX` copies the word at 0x1F81E to AX, then sets SP to 0xF820.

**NOTE**: The program above pops BX first, that's because it was pushed last.

**NOTE**: In 8086, `PUSH` & `POP` instructions can only work with 16-bit elements.



## Status register

### wikipedia [Status register](https://en.wikipedia.org/wiki/Status_register)




