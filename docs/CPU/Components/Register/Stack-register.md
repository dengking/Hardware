# [Stack register](https://en.wikipedia.org/wiki/Stack_register)

A **stack register** is a computer central [processor register](https://en.wikipedia.org/wiki/Processor_register) whose purpose is to keep track of a [call stack](https://en.wikipedia.org/wiki/Call_stack). On an [accumulator-based architecture](https://en.wikipedia.org/wiki/Accumulator-based_architecture) machine, this may be a dedicated register such as SP on an [Intel x86](https://en.wikipedia.org/wiki/Intel_x86) machine. 

## Stack registers in x86

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