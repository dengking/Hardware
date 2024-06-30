# Endianness

## Endianness in software engineering

本节总结在software engineering中的一些关于endian的内容，它能够启示我们，什么时候需要考虑endian。

### C-family language object representation

从C++和C标准来看，“endianness”决定了object的value的memory representation，关于memory representation，参见：

- 工程computer-arithmetic的`Bitwise-operation\Binary-representation`章节的内容。
- 关于object、value、和memory representation，在cppreference [Object#Object representation and value representation](https://en.cppreference.com/w/cpp/language/object#Object_representation_and_value_representation)中进行了详细说明，其中的object representation就是前面所说的memory representation。

### Sqlite file format

在[About SQLite](https://sqlite.org/about.html)中有这样的描述

> The database [file format](https://sqlite.org/fileformat2.html) is cross-platform - you can freely copy a database between 32-bit and 64-bit systems or between [big-endian](http://en.wikipedia.org/wiki/Endianness) and [little-endian](http://en.wikipedia.org/wiki/Endianness) architectures. These features make SQLite a popular choice as an [Application File Format](https://sqlite.org/appfileformat.html). 

### TODO C network library

在C network library中提供了endian转换函数。

## wikipedia [Endianness](https://en.wikipedia.org/wiki/Endianness)

In [computing](https://en.wikipedia.org/wiki/Computing), **endianness** refers to the order of [bytes](https://en.wikipedia.org/wiki/Byte) (or sometimes [bits](https://en.wikipedia.org/wiki/Bit)) within a [binary](https://en.wikipedia.org/wiki/Binary_number) representation of a number. It can also be used more generally to refer to the internal ordering of any representation, such as the digits in a [numeral system](https://en.wikipedia.org/wiki/Numeral_system) or the sections of a [date](https://en.wikipedia.org/wiki/Date_format_by_country).

> NOTE: 我们主要关注的是byte ordering

In its most common usage, endianness indicates the ordering of bytes within a multi-byte number. A **little-endian** ordering places the **least significant byte** first and the most significant byte last, while a **big-endian** ordering does the opposite. For example, consider the [unsigned](https://en.wikipedia.org/wiki/Signedness) [hexadecimal](https://en.wikipedia.org/wiki/Hexadecimal) number `0x1234`, which requires at least two bytes to represent. In a little-endian ordering, the bytes would be arranged `[ 0x34, 0x12 ]`, while in a big-endian ordering they would be `[ 0x12, 0x34 ]`.

> NOTE : `0x1234`表的是一个数，即我们前面所述的value，我们的认读顺序是从右往左，即`0X34`是最低有效位，而`0X12`是最高有效位；`[ 0x34, 0x12 ]`表示的是一个数组，顺序是从左往右，即它的第0个元素是`0X34`，第1个元素是`0x12`；显然， numeric [literals](https://en.wikipedia.org/wiki/Literal_(computer_programming)) 是big-endian，我们会把最高有效位放到低地址

Historically, various methods of endianness have been used in computing, including exotic forms such as middle-endianness. Today, however, **little-endianness** is the dominant ordering for **processor architectures** ([x86](https://en.wikipedia.org/wiki/X86), most [ARM](https://en.wikipedia.org/wiki/ARM_architecture) implementations) and their associated [memory](https://en.wikipedia.org/wiki/Computer_memory). Conversely, **big-endianness** is the dominant ordering in **networking protocols** ([IP](https://en.wikipedia.org/wiki/Internet_Protocol), [TCP](https://en.wikipedia.org/wiki/Transmission_Control_Protocol), [UDP](https://en.wikipedia.org/wiki/User_Datagram_Protocol)). [File formats](https://en.wikipedia.org/wiki/File_format) can use either ordering; in fact, some formats use a mixture of both.

> NOTE: 两者各有优势，在后面会对它们进行详细的分析说明；这就造成了它们在不同领域的application。

In [left-to-right scripts](https://en.wikipedia.org/wiki/Writing_system#Directionality), numbers are written with their digits in big-endian order. Similarly, programming languages use **big-endian digit ordering** for numeric [literals](https://en.wikipedia.org/wiki/Literal_(computer_programming)) as well as **big-endian language** (“left” and “right”) for [bit-shift](https://en.wikipedia.org/wiki/Bitwise_operation#Logical_shift) operations, regardless of the endianness of the target architecture. This can lead to confusion when interacting with little-endian numbers.

> NOTE: 上面这段话非常重要，它解答了我关于bitwise-operation和endian之间关系的疑惑，它总结得非常好。关于bitwise-operation，参见工程computer-arithmetic的`Bitwise-operation\Bitwise-operation.md`。
>
> 对于programmer而言的话，应该这样思考：我们在进行program的时候，我们是使用的value，我们是不需要关注这个value的memory representation的，也就是在大多数情况下，我们是不需要关注endian，正如前面所言““endianness”决定了value的memory representation。”，这个过程对于programmer而言是透明的，它往往是由compiler来实现的。



### Illustration

The following two descriptive illustrations assume a normal reading and writing convention of *left to right*, where the left-most digit or character therefore corresponds to data being sent or received first, or being in the *lowest address in memory,* and the right-most digit or character corresponds to the data being sent or received last, or being in the *highest* address in memory.

Big-endianness may be demonstrated by writing a decimal number, say one hundred twenty-three, on paper in the usual [positional notation](https://en.wikipedia.org/wiki/Positional_notation) understood by a numerate reader: *123*. The digits are written starting from the left and to the right, with the most significant digit, *1*, written first. This is analogous to the lowest [address of memory](https://en.wikipedia.org/wiki/Memory_address) being used first. This is an example of a big-endian convention taken from daily life.

The little-endian way of writing the same number, one hundred twenty-three, would place the hundreds-digit *1* in the right-most position: *321*. A person following conventional big-endian place-value order, who is not aware of this special ordering, would read a different number: three hundred and twenty one. Endianness in computing is similar, but it usually applies to the ordering of bytes, rather than of digits.

The illustrations to the right, where *a* is a memory address, show big-endian and little-endian storage in memory.

![big-endian](https://upload.wikimedia.org/wikipedia/commons/thumb/5/54/Big-Endian.svg/200px-Big-Endian.svg.png)

![little-endian](https://upload.wikimedia.org/wikipedia/commons/thumb/e/ed/Little-Endian.svg/200px-Little-Endian.svg.png)

### Hardware

[Computer memory](https://en.wikipedia.org/wiki/Computer_memory) consists of a sequence of storage cells. Each cell is identified in hardware and software by its [memory address](https://en.wikipedia.org/wiki/Memory_address). If the total number of storage cells in memory is *n*, then addresses are enumerated from *0* to *n-1*. Computer programs often use data structures of [fields](https://en.wikipedia.org/wiki/Field_(computer_science)) that may consist of more data than is stored in one memory cell. For the purpose of this article where its use as an operand of an instruction is relevant, a field consists of a consecutive sequence of [bytes](https://en.wikipedia.org/wiki/Byte) and represents a simple data value. In addition to that, it has to be of numeric type in some [positional number system](https://en.wikipedia.org/wiki/Positional_notation) (mostly base-10 or base-2 – or base-256 in case of 8-bit bytes).[[5\]](https://en.wikipedia.org/wiki/Endianness#cite_note-5) In such a number system the "value" of a digit is determined not only by its value as a single digit, but also by the position it holds in the complete number, its "significance". These positions can be mapped to memory mainly in two ways:[[6\]](https://en.wikipedia.org/wiki/Endianness#cite_note-TanenbaumAustin2012-6)

- increasing numeric significance with increasing memory addresses (or increasing time), known as *little-endian*, and
- decreasing numeric significance with increasing memory addresses (or increasing time), known as *big-endian*[[7\]](https://en.wikipedia.org/wiki/Endianness#cite_note-7)

#### Current architectures

The Intel [x86](https://en.wikipedia.org/wiki/X86) and also AMD64 / [x86-64](https://en.wikipedia.org/wiki/X86-64) series of processors use the **little-endian** format. Recently designed instruction set architectures typically follow this convention, either allowing only little-endian mode (e.g. [RISC-V](https://en.wikipedia.org/wiki/RISC-V), [Nios II](https://en.wikipedia.org/wiki/Nios_II), [Andes Technology](https://en.wikipedia.org/wiki/Andes_Technology)NDS32, or [Qualcomm Hexagon](https://en.wikipedia.org/wiki/Qualcomm_Hexagon)), or running mostly little-endian software on a bi-endian architecture (e.g. ARM [Aarch64](https://en.wikipedia.org/wiki/Aarch64), [C-Sky](https://en.wikipedia.org/w/index.php?title=C-Sky&action=edit&redlink=1)).

Some **big-endian** architectures that remain popular include mostly older examples like the IBM [z/Architecture](https://en.wikipedia.org/wiki/Z/Architecture), [Freescale ColdFire](https://en.wikipedia.org/wiki/Freescale_ColdFire) (which is [Motorola 68000 series](https://en.wikipedia.org/wiki/Motorola_68000_series)-based) and Atmel [AVR32](https://en.wikipedia.org/wiki/AVR32), but also the more recent [OpenRISC](https://en.wikipedia.org/wiki/OpenRISC). The [IBM AIX](https://en.wikipedia.org/wiki/IBM_AIX) and [Oracle Solaris](https://en.wikipedia.org/wiki/Oracle_Solaris) operating systems on **bi-endian** [Power ISA](https://en.wikipedia.org/wiki/Power_ISA) and [SPARC](https://en.wikipedia.org/wiki/SPARC) run in big-endian mode, while [Linux](https://en.wikipedia.org/wiki/Linux) on Power has moved to little-endian mode for new distributions.



#### Floating point

Although the ubiquitous x86 processors of today use little-endian storage for all types of data (integer, floating point, [BCD](https://en.wikipedia.org/wiki/Binary_coded_decimal)), there are a number of hardware architectures where [floating-point](https://en.wikipedia.org/wiki/Floating-point) numbers are represented in big-endian form while integers are represented in little-endian form.[[15\]](https://en.wikipedia.org/wiki/Endianness#cite_note-15) There are [ARM](https://en.wikipedia.org/wiki/ARM_architecture) processors that have half little-endian, half big-endian floating-point representation for double-precision numbers: both 32-bit words are stored in little-endian like integer registers, but the most significant one first. Because there have been many floating-point formats with no "[network](https://en.wikipedia.org/wiki/Computer_network)" standard representation for them, the [XDR](https://en.wikipedia.org/wiki/External_Data_Representation) standard uses big-endian IEEE 754 as its representation. It may therefore appear strange that the widespread [IEEE 754](https://en.wikipedia.org/wiki/IEEE_754) floating-point standard does not specify endianness.[[16\]](https://en.wikipedia.org/wiki/Endianness#cite_note-16) Theoretically, this means that even standard IEEE floating-point data written by one machine might not be readable by another. However, on modern standard computers (i.e., implementing IEEE 754), one may in practice safely assume that the endianness is the same for floating-point numbers as for integers, making the conversion straightforward regardless of data type. (Small [embedded systems](https://en.wikipedia.org/wiki/Embedded_system) using special floating-point formats may be another matter however.)

#### Optimization

The little-endian system has the property that the same **value** can be read from memory at **different lengths** without using **different addresses** (even when [alignment](https://en.wikipedia.org/wiki/Byte_alignment) restrictions are imposed). For example, a 32-bit memory location with content `4A 00 00 00` can be read at the same address as either [8-bit](https://en.wikipedia.org/wiki/8-bit) (value = `4A`), [16-bit](https://en.wikipedia.org/wiki/16-bit) (`004A`), [24-bit](https://en.wikipedia.org/wiki/24-bit) (`00004A`), or [32-bit](https://en.wikipedia.org/wiki/32-bit) (`0000004A`), all of which retain the same numeric value. Although this **little-endian** property is rarely used directly by high-level programmers, it is often employed by code optimizers as well as by [assembly language](https://en.wikipedia.org/wiki/Assembly_language) programmers. In more concrete terms, such optimizations are the equivalent of the following C code returning true on most little-endian systems:

> NOTE:  要理解上面这段话所表达的思想，需要搞清楚：value、memory representation、endian之间的关联，在工程programming-language的`C-family-language\C++\Language-reference\Basic-concept\Data-model\Object\Object.md`中，有过这方面的讨论：Type determines the interpretion of memory representation, and further determine value。下面结合endian进行更加深入的论述：
>
> 上述“a 32-bit memory location with content `4A 00 00 00` ”中的`4A 00 00 00`是memory representation：
>
> - 第一字节：`4A`
> - 第二字节：`00`
> - 第三字节：`00`
> - 第四字节：`00`
>
> 不同的endian下，它表示的value是不同的：
>
> |               | 最高/最低有效位            | memory representation in bit                                 | value                      |
> | ------------- | -------------------------- | ------------------------------------------------------------ | -------------------------- |
> | big endian    | 最高: `4A` <br>最低: `00`  | `0100 1010` <br>` 0000 0000`<br>`0000 0000`<br>`0000 0000`   | $2^{30} + 2^{27} + 2^{25}$ |
> | little endian | 最高: `00` <br/>最低: `4A` | `0100 1010` <br/>` 0000 0000`<br/>`0000 0000`<br/>`0000 0000` | $2^6 + 2^3 + 2^1$          |
>
> 通过上述解释，就能够理解原文中"The little-endian system has the property that the same **value** can be read from memory at **different lengths** without using **different addresses** (even when [alignment](https://en.wikipedia.org/wiki/Byte_alignment) restrictions are imposed). "的含义了，下面是我的直观的理解：
>
> 对于little endian，给定memory representation，CPU对memory representation的interpretion是可以渐进式地计算的：即从低地址开始逐步计算，因为**最低有效位**位于**低地址**，对于低地址的值的计算是不受后面的高地址的值的影响。
>
> 对于big endain，与little endian是正好相反的。
>
> 下面的代码，极好的展示了little endian的这种attribute：

```c
#include "stdint.h"
#include "stdio.h"

int main(void)
{
	union u_t
	{
		uint8_t u8;
		uint16_t u16;
		uint32_t u32;
		uint64_t u64;
	} u = { .u64 = 0x4A };
	puts(u.u8 == u.u16 && u.u8 == u.u32 && u.u8 == u.u64 ? "true" : "false");
}
// gcc test.c

```

> NOTE: `0x4A`位于低地址，它只需要一个字节即可。

While not allowed by C++, such [Type punning](https://en.wikipedia.org/wiki/Type_punning) code is allowed as "implementation-defined" by the C11 standard[[17\]](https://en.wikipedia.org/wiki/Endianness#cite_note-17) and commonly used[[18\]](https://en.wikipedia.org/wiki/Endianness#cite_note-18) in code interacting with hardware.[[19\]](https://en.wikipedia.org/wiki/Endianness#cite_note-19)

On the other hand, in some situations it may be useful to obtain an approximation of a multi-byte or multi-word value by reading only its most significant portion instead of the complete representation; a big-endian processor may read such an approximation using the same base-address that would be used for the full value.

> NOTE: 因为对于big-endian而言，它的most significant bit位于低地址

Optimizations of this kind are not portable across systems of different endianness.

#### Calculation order

Little-endian representation simplifies hardware in processors that add multi-byte integral values a byte at a time, such as small-scale byte-addressable processors and [microcontrollers](https://en.wikipedia.org/wiki/Microcontroller). As carry propagation（进位） must start at the **least significant bit** (and thus byte), multi-byte addition can then be carried out with a monotonically-incrementing address sequence, a simple operation already present in hardware. On a big-endian processor, its addressing unit has to be told how big the addition is going to be so that it can hop forward to the least significant byte, then count back down towards the most significant byte (**MSB**). On the other hand, arithmetic division is done starting from the **MSB**, so it is more natural for **big-endian** processors. However, high-performance processors usually fetch typical multi-byte operands from memory in the same amount of time they would have fetched a single byte, so the complexity of the hardware is not affected by the **byte ordering**.

### Mapping multi-byte binary values to memory

We can assume that as we write text *left to right*, we are increasing the 'address' on paper, as a processor would write bytes with increasing memory addresses — as in the adjacent table. On paper, the hex value `0a0b0c0d` (written 168496141 in usual decimal notation) is big-endian style since we write the most significant digit first and the rest follow in ***de*creasing** significance. Mapping this number as a binary value to a sequence of 4 bytes in memory in big-endian style also writes the bytes from *left to right* in ***de*creasing** significance: `0Ah` at +0, `0Bh` at +1, `0Ch` at +2, `0Dh` at +3.

On a little-endian system, the bytes are written from *left to right* in ***in**creasing* significance, starting with the one's byte: `0Dh` at +0, `0Ch` at +1, `0Bh` at +2, `0Ah`at +3. Writing a 32-bit binary value to a memory location on a little-endian system and outputting the memory location (with growing addresses from left to right) shows that the order is *reversed* (byte-swapped) compared to usual big-endian notation. This is the way a [hexdump](https://en.wikipedia.org/wiki/Hexdump) is displayed: because the dumping program is unable to know what kind of data it is dumping, the only orientation it can observe is monotonically increasing addresses. The human reader, however, who knows that they are reading a hexdump of a little-endian system and who knows what kind of data they are reading, reads the byte sequence `0Dh`,`0Ch`,`0Bh`,`0Ah` as the 32-bit binary value 168496141, or 0x0a0b0c0d in hexadecimal notation. (Of course, this is *not* the same as the 

### Examples

#### A C programming example

Consider the following [C language](https://en.wikipedia.org/wiki/C_(programming_language)) program:

```c
#include <stdio.h>
int main(void)
{
	union
	{
		unsigned int word;  // a 32-bit integer
		unsigned char bytes[4];
	} u;

	u.word = 0x0A0B0C0D;
	for (int i = 0; i < 4; i++)
	{
		printf("byte[%d] = 0x%x\n", i, (int) u.bytes[i]);
	}
}

```

On a little-endian computer, this program would output:

```c
   byte[0] = 0xd
   byte[1] = 0xc
   byte[2] = 0xb
   byte[3] = 0xa
```

On a big-endian computer, this program would output:

```c
   byte[0] = 0xa
   byte[1] = 0xb
   byte[2] = 0xc
   byte[3] = 0xd
```

### Files and byte swap

Endianness is a problem when a binary file created on a computer is read on another computer with different endianness. Some [CPU](https://en.wikipedia.org/wiki/CPU) instruction sets provide native support for endian byte swapping, such as *bswap*[[21\]](https://en.wikipedia.org/wiki/Endianness#cite_note-21) ([x86](https://en.wikipedia.org/wiki/X86) - [486](https://en.wikipedia.org/wiki/Intel_80486) and later), and *rev*[[22\]](https://en.wikipedia.org/wiki/Endianness#cite_note-22)([ARMv6](https://en.wikipedia.org/wiki/ARM_architecture) and later).

Some [compilers](https://en.wikipedia.org/wiki/Compiler) have built-in facilities to deal with data written in other formats. For example, the [Intel](https://en.wikipedia.org/wiki/Intel) [Fortran](https://en.wikipedia.org/wiki/Fortran) compiler supports the non-standard `CONVERT` specifier, so a file can be opened as

`OPEN(unit,CONVERT='BIG_ENDIAN',...)`

or

`OPEN(unit,CONVERT='LITTLE_ENDIAN',...)`

Some compilers have options to generate code that globally enables the conversion for all file IO operations. This allows programmers to reuse code on a system with the opposite endianness without having to modify the code itself. If the compiler does not support such conversion, the programmer needs to swap the bytes via **ad hoc**（特殊的，专门的） code.

[Unicode](https://en.wikipedia.org/wiki/Unicode) text can optionally start with a [byte order mark](https://en.wikipedia.org/wiki/Byte_order_mark) (BOM) to signal the endianness of the file or stream. Its code point is `U+FEFF`. In [UTF-32](https://en.wikipedia.org/wiki/UTF-32) for example, a big-endian file should start with `00 00 FE FF`; a little-endian should start with `FF FE 00 00`.

Application binary data formats, such as for example [MATLAB](https://en.wikipedia.org/wiki/MATLAB) .mat files, or the .BIL data format, used in topography, are usually endianness-independent. This is achieved by:

1. storing the data always in one fixed endianness, or
2. carrying with the data a switch to indicate which endianness the data was written with.

When reading the file, the application converts the endianness, invisibly from the user. An example of the first case is the binary [XLS file](https://en.wikipedia.org/wiki/XLS_file) format that is portable between Windows and Mac systems and always little-endian, leaving the Mac application to swap the bytes on load and save when running on a big-endian Motorola 68K or PowerPC processor.[[23\]](https://en.wikipedia.org/wiki/Endianness#cite_note-23)

[TIFF](https://en.wikipedia.org/wiki/TIFF) image files are an example of the second strategy, whose header instructs the application about endianness of their internal binary integers. If a file starts with the signature "`MM`" it means that integers are represented as big-endian, while "`II`" means little-endian. Those signatures need a single 16-bit word each, and they are [palindromes](https://en.wikipedia.org/wiki/Palindrome) (that is, they read the same forwards and backwards), so they are endianness independent. "`I`" stands for [Intel](https://en.wikipedia.org/wiki/Intel) and "`M`" stands for [Motorola](https://en.wikipedia.org/wiki/Motorola), the respective [CPU](https://en.wikipedia.org/wiki/CPU) providers of the [IBM PC](https://en.wikipedia.org/wiki/IBM_PC) compatibles (Intel) and [Apple Macintosh](https://en.wikipedia.org/wiki/Apple_Macintosh) platforms (Motorola) in the 1980s. Intel CPUs are little-endian, while Motorola 680x0 CPUs are big-endian. This explicit signature allows a TIFF reader program to swap bytes if necessary when a given file was generated by a TIFF writer program running on a computer with a different endianness.

Since the required byte swap depends on the size of the numbers stored in the file (two 2-byte integers require a different swap than one 4-byte integer), the file format must be known to perform endianness conversion.

```c
/* C function to change endianness for byte swap in an unsigned 32-bit integer */

uint32_t ChangeEndianness(uint32_t value)
{
    uint32_t result = 0;
    result |= (value & 0x000000FF) << 24;
    result |= (value & 0x0000FF00) << 8;
    result |= (value & 0x00FF0000) >> 8;
    result |= (value & 0xFF000000) >> 24;
    return result;
}
```

> NOTE :关于上面这段代码的细致分析，参见[Endianness conversion in C](https://codereview.stackexchange.com/questions/151049/endianness-conversion-in-c)

### File systems



### Networking

Many [IETF RFCs](https://en.wikipedia.org/wiki/IETF_RFC) use the term *network order*, meaning the order of transmission for bits and bytes *over the wire* in [network protocols](https://en.wikipedia.org/wiki/Network_protocols). Among others, the historic [RFC 1700](https://tools.ietf.org/html/rfc1700) (also known as [Internet standard](https://en.wikipedia.org/wiki/Internet_standard) STD 2) has defined the network order for protocols in the [Internet protocol suite](https://en.wikipedia.org/wiki/Internet_protocol_suite) to be [big-endian](https://en.wikipedia.org/wiki/Endianness#Big-endian), hence the use of the term "network byte order" for big-endian byte order.[[25\]](https://en.wikipedia.org/wiki/Endianness#cite_note-25)

However, not all protocols use big-endian byte order as the network order. The [Server Message Block](https://en.wikipedia.org/wiki/Server_Message_Block) (SMB) protocol uses little-endian byte order. In [CANopen](https://en.wikipedia.org/wiki/CANopen), multi-byte parameters are always sent [least significant byte](https://en.wikipedia.org/wiki/Least_significant_byte) first (little-endian). The same is true for [Ethernet Powerlink](https://en.wikipedia.org/wiki/Ethernet_Powerlink).[[26\]](https://en.wikipedia.org/wiki/Endianness#cite_note-26)

The [Berkeley sockets](https://en.wikipedia.org/wiki/Berkeley_sockets) [API](https://en.wikipedia.org/wiki/Application_programming_interface) defines a set of functions to convert 16-bit and 32-bit integers to and from network byte order: the `htons` (host-to-network-short) and `htonl` (host-to-network-long) functions convert 16-bit and 32-bit values respectively from machine (*host*) to network order; the `ntohs` and `ntohl` functions convert from network to host order. These functions may be a [no-op](https://en.wikipedia.org/wiki/No-op) on a big-endian system.

While the high-level network protocols usually consider the byte (mostly meant as *octet*) as their atomic unit, the lowest network protocols may deal with ordering of bits within a byte.



### Bit endianness

[Bit numbering](https://en.wikipedia.org/wiki/Bit_numbering) is a concept similar to endianness, but on a level of bits, not bytes. *Bit endianness* or *bit-level endianness* refers to the transmission order of bits over a serial medium. The bit-level analogue of little-endian (least significant bit goes first) is used in [RS-232](https://en.wikipedia.org/wiki/RS-232), [HDLC](https://en.wikipedia.org/wiki/HDLC), [Ethernet](https://en.wikipedia.org/wiki/Ethernet), and [USB](https://en.wikipedia.org/wiki/USB). Some protocols use the opposite ordering (e.g. [Teletext](https://en.wikipedia.org/wiki/Teletext), [I²C](https://en.wikipedia.org/wiki/I²C), [SMBus](https://en.wikipedia.org/wiki/SMBus), [PMBus](https://en.wikipedia.org/wiki/PMBus), and [SONET and SDH](https://en.wikipedia.org/wiki/Synchronous_optical_networking)[[27\]](https://en.wikipedia.org/wiki/Endianness#cite_note-27)). Usually, there exists a consistent view to the bits irrespective of their order in the byte, such that the latter becomes relevant only on a very low level. One exception is caused by the feature of some [cyclic redundancy checks](https://en.wikipedia.org/wiki/Cyclic_redundancy_check) to detect *all* [burst errors](https://en.wikipedia.org/wiki/Burst_error) up to a known length, which would be spoiled if the bit order is different from the byte order on serial transmission.

Apart from serialization, the terms *bit endianness* and *bit-level endianness* are seldom used, as computer architectures where each individual bit has a unique address are rare. Individual bits or [bit fields](https://en.wikipedia.org/wiki/Bit_field) are accessed via their numerical value or, in high-level programming languages, assigned names, the effects of which, however, may be machine dependent or lack [software portability](https://en.wikipedia.org/wiki/Software_portability). The natural numbering is that the [arithmetic left shift](https://en.wikipedia.org/wiki/Arithmetic_left_shift) `1 << *n*` yields a mask for the bit of position *n*, a rule which exhibits the machine's (byte) endianness at least if `*n* >= 8`, e.g. if used for indexing a sufficiently large bit array. Other numberings do occur in various documentations.

