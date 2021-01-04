# 关于本章

在上一节中给出了Memory–processor transfer的一个简单模型，本节提供memory alignment的角度对此进行详细介绍，通过这些分析，读者应该能够对此有更加深刻的认知。

## cse.bgu [Memory Alignment](http://www.cse.bgu.ac.il/common/download.asp?FileName=Memory%20Alignment.pdf&AppID=2&MainID=570&SecID=3014&MinID=2)

When a computer reads from or writes to a memory address, it will do this in **word** sized chunks (for example, 4 byte (32-bit) chunks on the MPC8360). **Data alignment** means putting the data at a memory offset equal to some multiple of the **word size**, which increases the system's performance due to the way the CPU handles memory. **Most CPUs can access only memory aligned addresses**. 

Table 1 show an example of some memory addresses and their alignment on different architectures. 

### Memory Alignment in the MPC8360 

The MPC8360 read/write operations are in 4 byte (32-bit) chunks. Thus, only memory addresses that are some multiple of four are considered aligned. The MPC8360 can access only aligned addresses. Reading a word (32 bit) from an aligned address in the MPC8360 (for example 0x0000_0008) can be achieved in a single load instruction. However, reading a word (32 bit) from an unaligned address (for example 0x0000_0009) will take at least two
read instructions. 

### Formal Definitions

A **memory address** `a`, is said to be n-byte aligned when `n` is a power of two and `a` is a multiple of `n` bytes. In this context **a byte** is the smallest unit of memory access, i.e. each memory address specifies a different byte. An n-byte aligned address would have $log2 n$ **least-significant zeros** when expressed in binary.

A memory access is said to be aligned when the data being accessed is n bytes long and the address is n-byte aligned. When a memory access is not aligned, it is said to be misaligned or Non Aligned. Note that by definition **byte memory accesses** are always aligned. 

