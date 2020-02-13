# [Memory Alignment](http://www.cse.bgu.ac.il/common/download.asp?FileName=Memory%20Alignment.pdf&AppID=2&MainID=570&SecID=3014&MinID=2)

When a computer reads from or writes to a memory address, it will do this in **word** sized
chunks (for example, 4 byte (32-bit) chunks on the MPC8360). **Data alignment** means putting
the data at a memory offset equal to some multiple of the **word size**, which increases the
system's performance due to the way the CPU handles memory. **Most CPUs can access only**
**memory aligned addresses**. 

Table 1 show an example of some memory addresses and their alignment on different
architectures. 

## Memory Alignment in the MPC8360 

The MPC8360 read/write operations are in 4 byte (32-bit) chunks. Thus, only memory
addresses that are some multiple of four are considered aligned. The MPC8360 can access
only aligned addresses. Reading a word (32 bit) from an aligned address in the MPC8360 (for
example 0x0000_0008) can be achieved in a single load instruction. However, reading a
word (32 bit) from an unaligned address (for example 0x0000_0009) will take at least two
read instructions. 

## Formal Definitions

A **memory address** `a`, is said to be n-byte aligned when `n` is a power of two and `a` is a
multiple of `n` bytes. In this context **a byte** is the smallest unit of memory access, i.e. each
memory address specifies a different byte. An n-byte aligned address would have $log2 n$
**least-significant zeros** when expressed in binary.

A memory access is said to be aligned when the data being accessed is n bytes long and the
address is n-byte aligned. When a memory access is not aligned, it is said to be misaligned or
Non Aligned. Note that by definition **byte memory accesses** are always aligned. 





## [Purpose of memory alignment](https://stackoverflow.com/questions/381244/purpose-of-memory-alignment)

The memory subsystem on a modern processor is restricted to accessing memory at the granularity and alignment of its word size; this is the case for a number of reasons.

### Speed

Modern processors have multiple levels of cache memory that data must be pulled through; supporting single-byte reads would make the memory subsystem throughput tightly bound to the execution unit throughput (aka cpu-bound); this is all reminiscent of how [PIO mode was surpassed by DMA](http://www.differencebetween.net/technology/difference-between-dma-and-pio/) for many of the same reasons in hard drives.

The CPU **always** reads at its word size (4 bytes on a 32-bit processor), so when you do an unaligned address access — on a processor that supports it — the processor is going to read multiple words. The CPU will read each word of memory that your requested address straddles. This causes an amplification of up to 2X the number of memory transactions required to access the requested data.

Because of this, it can very easily be slower to read two bytes than four. For example, say you have a struct in memory that looks like this:

```c
struct mystruct {
    char c;  // one byte
    int i;   // four bytes
    short s; // two bytes
}
```

On a 32-bit processor it would most likely be aligned like shown here:

![Struct Layout](https://i.stack.imgur.com/Vkg0j.png)

The processor can read each of these members in one transaction.

Say you had a packed version of the struct, maybe from the network where it was packed for transmission efficiency; it might look something like this:

![Packed Struct](https://i.stack.imgur.com/Ebcwt.png)

Reading the first byte is going to be the same.

When you ask the processor to give you 16 bits from 0x0005 it will have to read a word from from 0x0004 and shift left 1 byte to place it in a 16-bit register; some extra work, but most can handle that in one cycle.

When you ask for 32 bits from 0x0001 you'll get a 2X amplification. The processor will read from 0x0000 into the result register and shift left 1 byte, then read again from 0x0004 into a temporary register, shift right 3 bytes, then `OR` it with the result register.

### Range

For any given address space, if the architecture can assume that the 2 LSBs are always 0 (e.g., 32-bit machines) then it can access 4 times more memory (the 2 saved bits can represent 4 distinct states), or the same amount of memory with 2 bits for something like flags. Taking the 2 LSBs off of an address would give you a 4-byte alignment; also referred to as a [stride](http://en.wikipedia.org/wiki/Stride_of_an_array) of 4 bytes. Each time an address is incremented it is effectively incrementing bit 2, not bit 0, i.e., the last 2 bits will always continue to be `00`.

This can even affect the physical design of the system. If the address bus needs 2 fewer bits, there can be 2 fewer pins on the CPU, and 2 fewer traces on the circuit board.

### Atomicity

The CPU can operate on an aligned word of memory atomically, meaning that no other instruction can interrupt that operation. This is critical to the correct operation of many [lock-free data structures](http://kukuruku.co/hub/cpp/lock-free-data-structures-basics-atomicity-and-atomic-primitives) and other [concurrency](http://www.sciencedirect.com/science/article/pii/0304397588900965) paradigms.

### Conclusion

The memory system of a processor is quite a bit more complex and involved than described here; a discussion on [how an x86 processor actually addresses memory](http://www.rcollins.org/articles/pmbasics/tspec_a1_doc.html) can help (many processors work similarly).

There are many more benefits to adhering to memory alignment that you can read at [this IBM article](http://www.ibm.com/developerworks/library/pa-dalign/).

A computer's primary use is to transform data. Modern memory architectures and technologies have been optimized over decades to facilitate getting more data, in, out, and between more and faster execution units–in a highly reliable way.

### Bonus: Caches

Another alignment-for-performance that I alluded to previously is alignment on cache lines which are (for example, on some CPUs) 64B.

For more info on how much performance can be gained by leveraging caches, take a look at [Gallery of Processor Cache Effects](http://igoro.com/archive/gallery-of-processor-cache-effects/); from this [question on cache-line sizes](https://stackoverflow.com/questions/14707803/line-size-of-l1-and-l2-caches)

> Understanding of cache lines can be important for certain types of program optimizations. For example, alignment of data may determine whether an operation touches one or two cache lines. As we saw in the example above, this can easily mean that in the misaligned case, the operation will be twice slower.





## See also

[Why does CPU access memory on a word boundary?](https://stackoverflow.com/questions/3655926/why-does-cpu-access-memory-on-a-word-boundary)

[What does it mean by word size in computer?](https://stackoverflow.com/questions/19821103/what-does-it-mean-by-word-size-in-computer)