# 关于本章

本章描述Intel CPU的register。Intel CPU是在不断演进中的，我们以相对简单的x86为例来进行描述。

## 入门读物: skullsecurity [Registers](https://wiki.skullsecurity.org/Registers)

This section is the first section specific to assembly. So if you're reading through the full guide, get ready for some actual learning!

A register is like a variable, except that there are a fixed number of registers. Each register is a special spot in the CPU where a single value is stored. A register is the only place where **math** can be done (**addition**, **subtraction**, etc). Registers frequently hold pointers which reference memory. Movement of values between registers and memory is very common.

Intel assembly has **8** general purpose 32-bit registers: `eax`, `ebx`, `ecx`, `edx`, `esi`, `edi`, `ebp`, `esp`. Although any data can be moved between any of these registers, compilers commonly use the same registers for the same uses, and some instructions (such as multiplication and division) can only use the registers they're designed to use.

Different compilers may have completely different conventions on how the various registers are used. For the purposes of this document, I will discuss the most common compiler, Microsoft's.

### Volatility

Some registers are typically **volatile** across functions, and others remain unchanged. This is a feature of the compiler's standards and must be looked after in the code, registers are not preserved automatically (although in some assembly languages they are -- but not in `x86`). What that means is, when a function is called, there is no guarantee that **volatile registers** will retain their value when the function returns, and it's the function's responsibility to preserve non-volatile registers.

The conventions used by Microsoft's compiler are:

- **Volatile**: `ecx`, `edx`
- **Non-Volatile**: `ebx`, `esi`, `edi`, `ebp`
- **Special**: `eax`, `esp` (discussed later)



### General Purpose Registers

This section will look at the 8 general purpose registers on the `x86` architecture.

#### `eax`

`eax` is a 32-bit **general-purpose register** with two common uses: 

- to store the **return value** of a function 

- as a special register for certain calculations. 

It is technically a **volatile register**, since the value isn't preserved. Instead, its value is set to the **return value** of a function before a function returns. Other than `esp`, this is probably the most important register to remember for this reason. `eax` is also used specifically in certain calculations, such as **multiplication** and **division**, as a **special register**. That use will be examined in the instructions section.

Here is an example of a function returning in C:

```c
return 3;  // Return the value 3
```

Here's the same code in assembly:

```assembly
mov eax, 3 ; Set eax (the return value) to 3
ret        ; Return
```

TODO: 需要添加具体例子来进行实践

#### `ebx`

`ebx` is a non-volatile general-purpose register. It has no specific uses, but is often set to a commonly used value (such as 0) throughout a function to speed up calculations.



#### `ecx`

`ecx` is a **volatile general-purpose register** that is occasionally used as a function parameter or as a **loop counter**.

Functions of the "`__fastcall`" convention pass the first two parameters to a function using `ecx` and `edx`. Additionally, when calling a **member function** of a class, a **pointer** to that class is often passed in `ecx` no matter what the **calling convention** is.

Additionally, `ecx` is often used as a **loop counter**. *for* loops generally, although not always, set the accumulator variable to `ecx`. *rep-* instructions also use `ecx` as a counter, automatically decrementing it till it reaches 0. This class of function will be discussed in a later section.



#### `edx`

`edx` is a **volatile general-purpose register** that is occasionally used as a function parameter. Like `ecx`, `edx` is used for "`__fastcall`" functions.

Besides `fastcall`, `edx` is generally used for storing short-term variables within a function.

#### `esi`

`esi` is a **non-volatile general-purpose register** that is often used as a pointer. Specifically, for "rep-" class instructions, which require a source and a destination for data, `esi` points to the "source". `esi` often stores data that is used throughout a function because it doesn't change.

#### `edi`

`edi` is a **non-volatile general-purpose register** that is often used as a pointer. It is similar to `esi`, except that it is generally used as a destination for data.



#### `ebp`



`ebp` is a **non-volatile general-purpose register** that has two distinct uses depending on compile settings: it is either the **frame pointer** or a **general purpose register**.

If compilation is not optimized, or code is written by hand, `ebp` keeps track of where the stack is at the beginning of a function (the stack will be explained in great detail in a later section). Because the stack changes throughout a function, having `ebp` set to the original value allows variables stored on the stack to be referenced easily. This will be explored in detail when the stack is explained.

If compilation is optimized, `ebp` is used as a general register for storing any kind of data, while calculations for the **stack pointer** are done based on the **stack pointer** moving (which gets confusing -- luckily, IDA automatically detects and corrects a moving **stack pointer**!)





#### `esp`

`esp` is a special register that stores a pointer to the top of the stack (the top is actually at a **lower virtual address** than the bottom as the stack grows downwards in memory towards the **heap**). Math is rarely done directly on **`esp`**, and the value of **`esp`** must be the same at the beginning and the end of each function. `esp` will be examined in much greater detail in a later section.

### Special Purpose Registers

For special purpose and floating point registers not listed here, have a look at the [Wikipedia Article](http://en.wikipedia.org/wiki/IA-32) or other reference sites.

#### `eip`

*`eip`*, or the **instruction pointer**, is a special-purpose register which stores a pointer to the address of the instruction that is currently executing. Making a jump is like adding to or subtracting from the instruction pointer.

After each instruction, a value equal to the size of the instruction is added to `eip`, which means that `eip` points at the machine code for the next instruction. This simple example shows the automatic addition to `eip` at every step:

```c
eip+1      53                push    ebx
eip+4      8B 54 24 08       mov     edx, [esp+arg_0]
eip+2      31 DB             xor     ebx, ebx
eip+2      89 D3             mov     ebx, edx
eip+3      8D 42 07          lea     eax, [edx+7]
.....
```

### flags

In the **flags register**, each bit has a specific meaning and they are used to store meta-information about the results of previous operations. For example, whether the last calculation **overflowed** the register or whether the operands were equal. Our interest in the flags register is usually around the *cmp* and *test* operations which will commonly set or unset the zero, carry and overflow flags. These flags will then be tested by a conditional jump which may be controlling program flow or a loop.







## Canonical mnemonics for register 

### microsoft [x64 Architecture](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/x64-architecture)

| 64-bit register | Lower 32 bits | Lower 16 bits | Lower 8 bits |
| --------------- | ------------- | ------------- | ------------ |
| **rax**         | **eax**       | **ax**        | **al**       |
| **rbx**         | **ebx**       | **bx**        | **bl**       |
| **rcx**         | **ecx**       | **cx**        | **cl**       |
| **rdx**         | **edx**       | **dx**        | **dl**       |
| **rsi**         | **esi**       | **si**        | **sil**      |
| **rdi**         | **edi**       | **di**        | **dil**      |
| **rbp**         | **ebp**       | **bp**        | **bpl**      |
| **rsp**         | **esp**       | **sp**        | **spl**      |
| **r8**          | **r8d**       | **r8w**       | **r8b**      |
| **r9**          | **r9d**       | **r9w**       | **r9b**      |
| **r10**         | **r10d**      | **r10w**      | **r10b**     |
| **r11**         | **r11d**      | **r11w**      | **r11b**     |
| **r12**         | **r12d**      | **r12w**      | **r12b**     |
| **r13**         | **r13d**      | **r13w**      | **r13b**     |
| **r14**         | **r14d**      | **r14w**      | **r14b**     |
| **r15**         | **r15d**      | **r15w**      | **r15b**     |

### virginia [x86 Assembly Guide](https://www.cs.virginia.edu/~evans/cs216/guides/x86.html)

![](./virginia-x86-registers.png)

另外参见:

- skullsecurity [Registers#16-bit and 8-bit Registers](https://wiki.skullsecurity.org/Registers#16-bit_and_8-bit_Registers)
- utoronto [x86 Registers](https://www.eecg.utoronto.ca/~amza/www.mindsec.com/files/x86regs.html)

