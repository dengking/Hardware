# Integer 



## wikipedia [Integer (computer science)](https://en.wikipedia.org/wiki/Integer_(computer_science))

In computer science, an **integer** is a [datum](https://en.wikipedia.org/wiki/Data) of **integral data type**, a [data type](https://en.wikipedia.org/wiki/Data_type) that represents some [range](https://en.wikipedia.org/wiki/Interval_(mathematics)) of mathematical [integers](https://en.wikipedia.org/wiki/Integers). Integral data types may be of different sizes and may or may not be allowed to contain negative values. Integers are commonly represented in a computer as a group of binary digits (bits). The size of the grouping varies so the set of integer sizes available varies between different types of computers. Computer hardware, including [virtual machines](https://en.wikipedia.org/wiki/Virtual_machine), nearly always provide a way to represent a processor [register](https://en.wikipedia.org/wiki/Word_size) or memory address as an integer.



### Value and representation

The *value* of an item with an integral type is the mathematical integer that it corresponds to. Integral types may be *unsigned* (capable of representing only non-negative integers) or *signed* (capable of representing negative integers as well).[[1\]](https://en.wikipedia.org/wiki/Integer_(computer_science)#cite_note-1)

An integer value is typically specified in the [source code](https://en.wikipedia.org/wiki/Source_code) of a program as a sequence of digits optionally prefixed with + or −. Some programming languages allow other notations, such as hexadecimal (base 16) or octal (base 8). Some programming languages also permit [digit group separators](https://en.wikipedia.org/wiki/Digit_group_separator).[[2\]](https://en.wikipedia.org/wiki/Integer_(computer_science)#cite_note-2)

The *internal representation* of this datum is the way the value is stored in the computer's memory. Unlike mathematical integers, a typical datum in a computer has some minimal and maximum possible value.

The most common representation of a positive integer is a string of [bits](https://en.wikipedia.org/wiki/Bit), using the [binary numeral system](https://en.wikipedia.org/wiki/Binary_numeral_system). The order of the memory [bytes](https://en.wikipedia.org/wiki/Byte) storing the bits varies; see [endianness](https://en.wikipedia.org/wiki/Endianness). The *width* or *precision* of an integral type is the number of bits in its representation. An integral type with *n* bits can encode $2^n$ numbers; for example an unsigned type typically represents the non-negative values 0 through $2^{n−1}$. Other encodings of integer values to bit patterns are sometimes used, for example [binary-coded decimal](https://en.wikipedia.org/wiki/Binary-coded_decimal) or [Gray code](https://en.wikipedia.org/wiki/Gray_code), or as printed character codes such as [ASCII](https://en.wikipedia.org/wiki/ASCII).

There are four well-known [ways to represent signed numbers](https://en.wikipedia.org/wiki/Signed_number_representations) in a binary computing system. The most common is [two's complement](https://en.wikipedia.org/wiki/Two's_complement), which allows a signed integral type with *n* bits to represent numbers from $−2^{(n−1)}$ through $2^{(n−1)}−1$. Two's complement arithmetic is convenient because there is a perfect [one-to-one correspondence](https://en.wikipedia.org/wiki/Bijection) between representations and values (in particular, no separate +0 and −0), and because [addition](https://en.wikipedia.org/wiki/Addition), [subtraction](https://en.wikipedia.org/wiki/Subtraction) and [multiplication](https://en.wikipedia.org/wiki/Multiplication) do not need to distinguish between signed and unsigned types. Other possibilities include [offset binary](https://en.wikipedia.org/wiki/Offset_binary), [sign-magnitude](https://en.wikipedia.org/wiki/Sign-magnitude), and [ones' complement](https://en.wikipedia.org/wiki/Ones'_complement).

Some computer languages define integer sizes in a machine-independent way; others have varying definitions depending on the underlying processor word size. Not all language implementations define variables of all integer sizes, and defined sizes may not even be distinct in a particular implementation. An integer in one [programming language](https://en.wikipedia.org/wiki/Programming_language) may be a different size in a different language or on a different processor.

