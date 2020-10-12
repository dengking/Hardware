# x86 memory segmentation

x86 memory segmentation已经被淘汰了，但是在很多的文章，或者书籍中还是会见到它的身影，所以总结一下。

## 维基百科[x86 memory segmentation](https://en.wikipedia.org/wiki/X86_memory_segmentation)

**x86 memory segmentation** refers to the implementation of [memory segmentation](https://en.wikipedia.org/wiki/Memory_segmentation) in the Intel [x86](https://en.wikipedia.org/wiki/X86) computer [instruction set architecture](https://en.wikipedia.org/wiki/Instruction_set_architecture). Segmentation was introduced on the [Intel 8086](https://en.wikipedia.org/wiki/Intel_8086) in 1978 as a way to allow programs to address more than 64 KB (65,536 [bytes](https://en.wikipedia.org/wiki/Byte)) of memory. The [Intel 80286](https://en.wikipedia.org/wiki/Intel_80286) introduced a second version of segmentation in 1982 that added support for [virtual memory](https://en.wikipedia.org/wiki/Virtual_memory) and [memory protection](https://en.wikipedia.org/wiki/Memory_protection). At this point the original model was renamed **real mode**, and the new version was named **protected mode**. The [x86-64](https://en.wikipedia.org/wiki/X86-64) architecture, introduced in 2003, has largely **dropped** support for segmentation in 64-bit mode.



In both real and protected modes, the system uses 16-bit *segment registers* to derive the actual memory address. In real mode, the registers CS, DS, SS, and ES point to the currently used program [code segment](https://en.wikipedia.org/wiki/Code_segment) (CS), the current [data segment](https://en.wikipedia.org/wiki/Data_segment) (DS), the current [stack segment](https://en.wikipedia.org/wiki/Call_stack) (SS), and one *extra* segment determined by the programmer (ES). The [Intel 80386](https://en.wikipedia.org/wiki/Intel_80386), introduced in 1985, adds two additional segment registers, FS and GS, with no specific uses defined by the hardware. The way in which the **segment registers** are used differs between the two modes. 



The choice of segment is normally defaulted by the processor according to the function being executed. Instructions are always fetched from the **code segment**. Any stack push or pop or any data reference referring to the stack uses the **stack segment**. All other references to data use the **data segment**. The extra segment is the default destination for string operations (for example MOVS or CMPS). FS and GS have no hardware-assigned uses. The instruction format allows an optional *segment prefix* byte which can be used to override the default segment for selected instructions if desired. 



### Later developments

The [x86-64](https://en.wikipedia.org/wiki/X86-64) architecture does not use segmentation in long mode (64-bit mode). Four of the segment registers, CS, SS, DS, and ES, are forced to 0, and the limit to 264. 



##  `cs` and  `eip`

如果采用memory segmentation的话，则需要使用`cs`和`eip`来指定next instruction。

参见

- 4.2.4. Hardware Handling of Interrupts and Exceptions



## `ss` and `esp`

如果采用memory segmentation的话，则需要使用`ss`和`esp`来指定[call stack](https://en.wikipedia.org/wiki/Call_stack) of currently executed program。

参见：

- 维基百科[Stack registers in x86](https://en.wikipedia.org/wiki/Stack_register)