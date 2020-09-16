# INT (x86 instruction)

## 维基百科[INT (x86 instruction)](https://en.wikipedia.org/wiki/INT_(x86_instruction))





## felixcloutier [INT n/INTO/INT3/INT1 — Call to Interrupt Procedure](https://www.felixcloutier.com/x86/intn:into:int3:int1)

| Opcode  | Instruction | Op/En | 64-Bit Mode | Compat/Leg Mode | Description                                                  |
| ------- | ----------- | ----- | ----------- | --------------- | ------------------------------------------------------------ |
| CC      | INT3        | ZO    | Valid       | Valid           | Generate breakpoint trap.                                    |
| CD *ib* | INT *imm8*  | I     | Valid       | Valid           | Generate software interrupt with vector specified by immediate byte. |
| CE      | INTO        | ZO    | Invalid     | Valid           | Generate overflow trap if overflow flag is 1.                |
| F1      | INT1        | ZO    | Valid       | Valid           | Generate debug trap.                                         |

思考：`INT1` VS `INT3`？