# [Program counter](https://en.wikipedia.org/wiki/Program_counter)

这个寄存器非常重要，它告诉CPU去执行哪一条指令。所有对 [control flow](https://en.wikipedia.org/wiki/Control_flow) 的操作的指令最终都是通过操作这个寄存器的值来实现的。[Program counter](https://en.wikipedia.org/wiki/Program_counter)所指向的肯定是code area，[Program counter](https://en.wikipedia.org/wiki/Program_counter)相当于next pointer，默认情况下它是自加1的，除非通过[JMP (x86 instruction)](https://en.wikipedia.org/wiki/JMP_(x86_instruction))等指令来显示更改它的值。



