# 关于本章

[x86](https://en.wikipedia.org/wiki/X86)是比较常见的 [instruction set architecture](https://en.wikipedia.org/wiki/Instruction_set_architecture)，本章整理它的相关知识。



# 入门读物: virginia [x86 Assembly Guide](http://www.cs.virginia.edu/~evans/cs216/guides/x86.html)

## Memory and Addressing Modes

### Declaring Static Data Regions

You can declare static data regions (analogous to global variables) in x86 assembly using special assembler directives for this purpose. Data declarations should be preceded by the `.DATA` directive. Following this directive, the directives `DB`, `DW`, and `DD` can be used to declare one, two, and four byte data locations, respectively. Declared locations can be labeled with names for later reference — this is similar to declaring variables by name, but abides by some lower level rules. For example, locations declared in sequence will be located in memory next to one another.
Example declarations:
```assembly
.DATA			
var	 DB 64  	; Declare a byte, referred to as location var, containing the value 64.
var2 DB ?	; Declare an uninitialized byte, referred to as location var2.
     DB 10	; Declare a byte with no label, containing the value 10. Its location is var2 + 1.
X	 DW ?	; Declare a 2-byte uninitialized value, referred to as location X.
Y	 DD 30000    	; Declare a 4-byte value, referred to as location Y, initialized to 30000.
```

Unlike in high level languages where arrays can have many dimensions and are accessed by indices, arrays in x86 assembly language are simply a number of cells located contiguously in memory. An array can be declared by just listing the values, as in the first example below. Two other common methods used for declaring arrays of data are the `DUP` directive and the use of string literals. The `DUP` directive tells the assembler to duplicate an expression a given number of times. For example, `4 DUP(2)` is equivalent to `2, 2, 2, 2`.

Some examples:
```assembly
Z	    DD 1, 2, 3	; Declare three 4-byte values, initialized to 1, 2, and 3. The value of location Z + 8 will be 3.
bytes  	DB 10 DUP(?)	; Declare 10 uninitialized bytes starting at location bytes.
arr	    DD 100 DUP(0)    	; Declare 100 4-byte words starting at location arr, all initialized to 0
str	    DB 'hello',0	; Declare 6 bytes starting at the address str, initialized to the ASCII character values for hello and the null (0) byte.
```

### Addressing Memory

Modern x86-compatible processors are capable of addressing up to 232 bytes of memory: memory addresses are 32-bits wide. In the examples above, where we used **labels** to refer to memory regions, these **labels** are actually replaced by the assembler with 32-bit quantities that specify addresses in memory. In addition to supporting referring to memory regions by **labels** (i.e. constant values), the x86 provides a flexible scheme for computing and referring to memory addresses: up to two of the 32-bit registers and a 32-bit signed constant can be added together to compute a memory address. One of the registers can be optionally pre-multiplied by 2, 4, or 8.

The addressing modes can be used with many x86 instructions (we'll describe them in the next section). Here we illustrate some examples using the `mov` instruction that moves data between registers and memory. This instruction has two operands: the first is the destination and the second specifies the source.

Some examples of mov instructions using address computations are:
```assembly
mov  eax,   [ebx]	; Move the 4 bytes in memory at the address contained in EBX into EAX
mov  [var], ebx	; Move the contents of EBX into the 4 bytes at memory address var. (Note, var is a 32-bit constant).
mov  eax,   [esi-4]	; Move 4 bytes at memory address ESI + (-4) into EAX
mov  [esi+eax], cl	; Move the contents of CL into the byte at address ESI+EAX
mov  edx, [esi+4*ebx]    	; Move the 4 bytes of data at address ESI+4*EBX into EDX
```
> NOTE: `mov` 指令可以看做是: move to `DES` from `SRC`

Some examples of invalid address calculations include:
```assembly
mov  eax,           [ebx-ecx]	; Can only add register values
mov  [eax+esi+edi], ebx    	; At most 2 registers in address computation

```
### Size Directives

In general, the intended size of the of the data item at a given memory address can be inferred from the assembly code instruction in which it is referenced. For example, in all of the above instructions, the size of the memory regions could be inferred from the size of the **register operand**. When we were loading a 32-bit register, the assembler could infer that the region of memory we were referring to was 4 bytes wide. When we were storing the value of a one byte register to memory, the assembler could infer that we wanted the address to refer to a single byte in memory.

However, in some cases the size of a referred-to memory region is ambiguous. Consider the instruction `mov [ebx], 2`. Should this instruction move the value 2 into the single byte at address `EBX`? Perhaps it should move the 32-bit integer representation of 2 into the 4-bytes starting at address `EBX`. Since either is a valid possible interpretation, the assembler must be explicitly directed as to which is correct. The size directives `BYTE PTR`, `WORD PTR`, and `DWORD PTR` serve this purpose, indicating sizes of 1, 2, and 4 bytes respectively.

For example:
```assembly
mov BYTE PTR [ebx], 2	; Move 2 into the single byte at the address stored in EBX.
mov WORD PTR [ebx], 2	; Move the 16-bit integer representation of 2 into the 2 bytes starting at the address in EBX.
mov DWORD PTR [ebx], 2  ; Move the 32-bit integer representation of 2 into the 4 bytes starting at the address in EBX.

```
## Instructions

Machine instructions generally fall into three categories: 

- data movement

- arithmetic/logic

- control-flow. 

In this section, we will look at important examples of x86 instructions from each category. This section should not be considered an exhaustive list of x86 instructions, but rather a useful subset. For a complete list, see Intel's instruction set reference.
We use the following notation:
```assembly
<reg32> Any 32-bit register (EAX, EBX, ECX, EDX, ESI, EDI, ESP, or EBP)
<reg16>	Any 16-bit register (AX, BX, CX, or DX)
<reg8>	Any 8-bit register (AH, BH, CH, DH, AL, BL, CL, or DL)
<reg>	Any register
<mem>	A memory address (e.g., [eax], [var + 4], or dword ptr [eax+ebx])
<con32>	Any 32-bit constant
<con16>	Any 16-bit constant
<con8>	Any 8-bit constant
<con>	Any 8-, 16-, or 32-bit constant
```

### Data Movement Instructions

#### `mov`

**`mov` — Move (Opcodes: 88, 89, 8A, 8B, 8C, 8E, ...)**

The `mov` instruction copies the data item referred to by its second operand (i.e. register contents, memory contents, or a constant value) into the location referred to by its first operand (i.e. a register or memory). While register-to-register moves are possible, **direct memory-to-memory moves are not**. In cases where memory transfers are desired, the source memory contents must first be loaded into a register, then can be stored to the destination memory address.

Syntax
```assembly
mov <reg>,<reg>
mov <reg>,<mem>
mov <mem>,<reg>
mov <reg>,<const>
mov <mem>,<const>
```

Examples
```assembly
mov eax, ebx — copy the value in ebx into eax
mov byte ptr [var], 5 — store the value 5 into the byte at location var
```

#### `push`

**`push` — Push stack (Opcodes: FF, 89, 8A, 8B, 8C, 8E, ...)**

The `push` instruction places its operand onto the top of the hardware supported stack in memory. Specifically, `push` first decrements `ESP` by 4, then places its operand into the contents of the 32-bit location at address `[ESP]`. `ESP` (the stack pointer) is decremented by `push` since the x86 stack grows down - i.e. the stack grows from high addresses to lower addresses.

Syntax
```assembly
push <reg32>
push <mem>
push <con32>
```
Examples
```assembly
push eax — push eax on the stack
push [var] — push the 4 bytes at address var onto the stack
```
#### `pop`

**`pop` — Pop stack**

The `pop` instruction removes the 4-byte data element from the top of the hardware-supported stack into the specified operand (i.e. register or memory location). It first moves the 4 bytes located at memory location `[SP]` into the specified register or memory location, and then increments `SP` by 4.

Syntax
```assembly
pop <reg32>
pop <mem>
```

Examples
```assembly
pop edi — pop the top element of the stack into EDI.
pop [ebx] — pop the top element of the stack into memory at the four bytes starting at location EBX.
```
#### `lea`

`lea` — Load effective address

The `lea` instruction places the address specified by its second operand into the register specified by its first operand. Note, the contents of the memory location are not loaded, only the effective address is computed and placed into the register. This is useful for obtaining a pointer into a memory region.

Syntax
```assembly
lea <reg32>,<mem>
```

Examples
```assembly
lea edi, [ebx+4*esi] — the quantity EBX+4*ESI is placed in EDI.
lea eax, [var] — the value in var is placed in EAX.
lea eax, [val] — the value val is placed in EAX.
```
### Arithmetic and Logic Instructions

#### `add`

**`add` — Integer Addition**

The `add` instruction adds together its two operands, storing the result in its first operand. Note, whereas both operands may be registers, at most one operand may be a memory location.

Syntax
```assembly
add <reg>,<reg>
add <reg>,<mem>
add <mem>,<reg>
add <reg>,<con>
add <mem>,<con>
```
Examples
```
add eax, 10 — EAX ← EAX + 10
add BYTE PTR [var], 10 — add 10 to the single byte stored at memory address var
```
#### `sub`

**`sub` — Integer Subtraction**

The `sub` instruction stores in the value of its first operand the result of subtracting the value of its second operand from the value of its first operand. As with `add`

Syntax

```c++
sub <reg>,<reg>
sub <reg>,<mem>
sub <mem>,<reg>
sub <reg>,<con>
sub <mem>,<con>
```
Examples

```
sub al, ah — AL ← AL - AH
sub eax, 216 — subtract 216 from the value stored in EAX
```

#### `inc`, `dec`

**`inc`, `dec` — Increment, Decrement**

The `inc` instruction increments the contents of its operand by one. The `dec` instruction decrements the contents of its operand by one.

Syntax
```assembly
inc <reg>
inc <mem>
dec <reg>
dec <mem>
```
Examples
```assembly
dec eax — subtract one from the contents of EAX.
inc DWORD PTR [var] — add one to the 32-bit integer stored at location var
```
#### `imul`

**`imul` — Integer Multiplication**

The `imul` instruction has two basic formats: two-operand (first two syntax listings above) and three-operand (last two syntax listings above).

The two-operand form multiplies its two operands together and stores the result in the first operand. The result (i.e. first) operand must be a register.

The three operand form multiplies its second and third operands together and stores the result in its first operand. Again, the result operand must be a register. Furthermore, the third operand is restricted to being a constant value.

Syntax
```assembly
imul <reg32>,<reg32>
imul <reg32>,<mem>
imul <reg32>,<reg32>,<con>
imul <reg32>,<mem>,<con>
```
Examples
```assembly
imul eax, [var] — multiply the contents of EAX by the 32-bit contents of the memory location var. Store the result in EAX.
imul esi, edi, 25 — ESI → EDI * 25
```

#### `idiv`

**`idiv` — Integer Division**

The `idiv` instruction divides the contents of the 64 bit integer `EDX:EAX` (constructed by viewing `EDX` as the most significant four bytes and `EAX` as the least significant four bytes) by the specified operand value. The quotient result of the division is stored into `EAX`, while the remainder is placed in `EDX`.
Syntax
```assembly
idiv <reg32>
idiv <mem>
```
Examples
```assembly
idiv ebx — divide the contents of EDX:EAX by the contents of EBX. Place the quotient in EAX and the remainder in EDX.
idiv DWORD PTR [var] — divide the contents of EDX:EAX by the 32-bit value stored at memory location var. Place the quotient in EAX and the remainder in EDX.
```

#### `and`, `or`, `xor` 

**`and`, `or`, `xor` — Bitwise logical and, or and exclusive or**

These instructions perform the specified logical operation (logical bitwise `and`, `or`, and `exclusive or`, respectively) on their operands, placing the result in the first operand location.

Syntax
```
and <reg>,<reg>
and <reg>,<mem>
and <mem>,<reg>
and <reg>,<con>
and <mem>,<con>
or <reg>,<reg>
or <reg>,<mem>
or <mem>,<reg>
or <reg>,<con>
or <mem>,<con>
xor <reg>,<reg>
xor <reg>,<mem>
xor <mem>,<reg>
xor <reg>,<con>
xor <mem>,<con>
```
Examples
```
and eax, 0fH — clear all but the last 4 bits of EAX.
xor edx, edx — set the contents of EDX to zero.
```
#### `not`

**`not` — Bitwise Logical Not**

Logically negates the operand contents (that is, flips all bit values in the operand).

Syntax
```assembly
not <reg>
not <mem>
```
Example
```assembly
not BYTE PTR [var] — negate all bits in the byte at the memory location var.
```

#### `neg`

**`neg` — Negate**
Performs the two's complement negation of the operand contents.

Syntax

```assembly
neg <reg>
neg <mem>
```
Example
```assembly
neg eax — EAX → - EAX
```

#### `shl`, `shr` 

**`shl`, `shr` — Shift Left, Shift Right**

These instructions shift the bits in their first operand's contents left and right, padding the resulting empty bit positions with zeros. The shifted operand can be shifted up to 31 places. The number of bits to shift is specified by the second operand, which can be either an 8-bit constant or the register CL. In either case, shifts counts of greater then 31 are performed modulo 32.

Syntax
```assembly
shl <reg>,<con8>
shl <mem>,<con8>
shl <reg>,<cl>
shl <mem>,<cl>

shr <reg>,<con8>
shr <mem>,<con8>
shr <reg>,<cl>
shr <mem>,<cl>
```
Examples
```assembly
shl eax, 1 — Multiply the value of EAX by 2 (if the most significant bit is 0)
shr ebx, cl — Store in EBX the floor of result of dividing the value of EBX by 2n wheren is the value in CL.
```

### Control Flow Instructions

The x86 processor maintains an instruction pointer (`IP`) register that is a 32-bit value indicating the location in memory where the current instruction starts. Normally, it increments to point to the next instruction in memory begins after execution an instruction. The `IP` register cannot be manipulated directly, but is updated implicitly by provided control flow instructions.
We use the notation `<label>` to refer to labeled locations in the program text. Labels can be inserted anywhere in x86 assembly code text by entering a label name followed by a colon. For example,

```assembly
		mov esi, [ebp+8]
begin:  xor ecx, ecx
        mov eax, [esi]
```
The second instruction in this code fragment is labeled `begin`. Elsewhere in the code, we can refer to the memory location that this instruction is located at in memory using the more convenient symbolic name begin. This label is just a convenient way of expressing the location instead of its 32-bit value.

#### `jmp`

**`jmp` — Jump**

Transfers program control flow to the instruction at the memory location indicated by the operand.

Syntax
```
jmp <label>
```
Example
```
jmp begin — Jump to the instruction labeled begin.
```

#### `jcondition`

**`jcondition` — Conditional Jump**

These instructions are conditional jumps that are based on the status of a set of condition codes that are stored in a special register called the machine status word. The contents of the machine status word include information about the last arithmetic operation performed. For example, one bit of this word indicates if the last result was zero. Another indicates if the last result was negative. Based on these condition codes, a number of conditional jumps can be performed. For example, the jz instruction performs a jump to the specified operand label if the result of the last arithmetic operation was zero. Otherwise, control proceeds to the next instruction in sequence.

A number of the conditional branches are given names that are intuitively based on the last operation performed being a special compare instruction, cmp (see below). For example, conditional branches such as jle and jne are based on first performing a cmp operation on the desired operands.

Syntax
```assembly
je <label> (jump when equal)
jne <label> (jump when not equal)
jz <label> (jump when last result was zero)
jg <label> (jump when greater than)
jge <label> (jump when greater than or equal to)
jl <label> (jump when less than)
jle <label> (jump when less than or equal to)
```
Example
```assembly
cmp eax, ebx
jle done
```
If the contents of `EAX` are less than or equal to the contents of `EBX`, jump to the label `done`. Otherwise, continue to the next instruction.

#### `cmp`

`cmp` — Compare

Compare the values of the two specified operands, setting the condition codes in the machine status word appropriately. This instruction is equivalent to the sub instruction, except the result of the subtraction is discarded instead of replacing the first operand.

Syntax
```assembly
cmp <reg>,<reg>
cmp <reg>,<mem>
cmp <mem>,<reg>
cmp <reg>,<con>
```
Example
```assembly
cmp DWORD PTR [var], 10
jeq loop
```
If the 4 bytes stored at location `var` are equal to the 4-byte integer constant 10, jump to the location labeled `loop`.

#### `call`, `ret`

`call`, `ret` — Subroutine call and return

These instructions implement a subroutine call and return. The `call` instruction first pushes the current code location onto the hardware supported stack in memory (see the `push` instruction for details), and then performs an unconditional jump to the code location indicated by the label operand. Unlike the simple jump instructions, the call instruction saves the location to return to when the subroutine completes.

The `ret` instruction implements a subroutine return mechanism. This instruction first pops a code location off the hardware supported in-memory stack (see the pop instruction for details). It then performs an unconditional jump to the retrieved code location.

Syntax

```
call <label>
ret
```



[Assembly Language Tutorial](https://wiki.skullsecurity.org/index.php?title=Assembly)