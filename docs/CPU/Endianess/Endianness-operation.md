# Endianness operation

需要考虑在哪些情况，programmer是需要考虑endian的，哪些情况是不需要考虑的。

下面罗列了需要考虑的典型场景：

- 计算hash值（参见 quarkslab [Unaligned accesses in C/C++: what, why and solutions to do it properly](https://blog.quarkslab.com/unaligned-accesses-in-cc-what-why-and-solutions-to-do-it-properly.html)）
- 网络传输：网络数据采用的是big endian，发送时，需要将其转换为big endian，接收时，需要将其从big endian转换为主句的endian。

下面罗列了不需要考虑的典型场景：

- literal默认是big endian，所以literal、bit-shift operation是不需要考虑endian的（参见wikipedia [Endianness](https://en.wikipedia.org/wiki/Endianness)）

## Conversion

### stackexchange [Endianness conversion in C](https://codereview.stackexchange.com/questions/151049/endianness-conversion-in-c)

I have written a simple C header for converting the endianness of `short` integers and `long` integers. It uses the GCC macro `__BYTE_ORDER__` to check the system's byte order and define the macros based on that.

The header creates the macros `LITTLE_ENDIAN_SHORT(n)`, `LITTLE_ENDIAN_LONG(n)`, `BIG_ENDIAN_SHORT(n)`, `BIG_ENDIAN_LONG(n)` which convert the value `n` from host endianness to the endianness specified.

Here is the source for `endian.h`:

```c
#ifndef ENDIAN_H
#define ENDIAN_H

#define REVERSE_SHORT(n) ((unsigned short) (((n & 0xFF) << 8) | \
                                            ((n & 0xFF00) >> 8)))
#define REVERSE_LONG(n) ((unsigned long) (((n & 0xFF) << 24) | \
                                          ((n & 0xFF00) << 8) | \
                                          ((n & 0xFF0000) >> 8) | \
                                          ((n & 0xFF000000) >> 24)))

#if __BYTE_ORDER__ == __ORDER_LITTLE_ENDIAN__
#    define LITTLE_ENDIAN_SHORT(n) (n)
#    define LITTLE_ENDIAN_LONG(n) (n)
#    define BIG_ENDIAN_SHORT(n) REVERSE_SHORT(n)
#    define BIG_ENDIAN_LONG(n) REVERSE_LONG(n)
#elif __BYTE_ORDER__ == __ORDER_BIG_ENDIAN__
#    define LITTLE_ENDIAN_SHORT(n) REVERSE_SHORT(n)
#    define LITTLE_ENDIAN_LONG(n) REVERSE_LONG(n)
#    define BIG_ENDIAN_SHORT(n) (n)
#    define BIG_ENDIAN_LONG(n) (n)
#else
#    error unsupported endianness
#endif

#endif
```

Is this a good way to implement the macros or is there a better way?



***COMMENTS*** 

- 1

- You may want to take a look at [codereview.stackexchange.com/a/149751/4203](http://codereview.stackexchange.com/a/149751/4203) – [forsvarir](https://codereview.stackexchange.com/users/4203/forsvarir) [Dec 28 '16 at 14:29](https://codereview.stackexchange.com/questions/151049/endianness-conversion-in-c#comment284410_151049)

- 3

  A better way to implement the macros would be to replace them with `inline` functions. :-) – [Cody Gray](https://codereview.stackexchange.com/users/121675/cody-gray) [Dec 28 '16 at 18:24](https://codereview.stackexchange.com/questions/151049/endianness-conversion-in-c#comment284450_151049)

- 

  It should be `0xFF000000U` (and for good measure, you can add `U` to the other bitmasks as well). While most compilers do the right thing, you are technically overflowing a signed integer here. – [Simon Richter](https://codereview.stackexchange.com/users/7960/simon-richter) [Dec 29 '16 at 3:16](https://codereview.stackexchange.com/questions/151049/endianness-conversion-in-c#comment284571_151049)

- 1

  Is there some reason why you want to roll your own endian-conversion code rather than using the standard `htonl()`/`ntohl()` and `htons()`/`ntohs()` functions? The latter are much harder to mess up and easier for other programmers to read/understand/trust... – [Jeremy Friesner](https://codereview.stackexchange.com/users/126857/jeremy-friesner) [Dec 29 '16 at 5:29](https://codereview.stackexchange.com/questions/151049/endianness-conversion-in-c#comment284583_151049)



#### [A](https://codereview.stackexchange.com/a/151070)

First off, [Jean-François is absolutely right](https://codereview.stackexchange.com/a/151059/121675): you cannot assume any particular bit widths for the built-in types, `short`, `int`, `long`, etc. Use the types defined in `stdint.h` that have explicit bit widths to ensure that the code is correct and portable.

Otherwise, your code looks pretty good, and this is a reasonable implementation. But…

> Is this a good way to implement the macros or is there a better way?

There is indeed a better way: to use `inline` functions rather than macros. :-)

After an optimizing compiler finishes, the resulting code will be equivalent, but the advantages of functions are numerous: type safety, the ability to use expressions as arguments, no insidious parenthesization bugs, etc.

You will still need some preprocessor magic to avoid generating code when it is not necessary, but this is still much cleaner than implementing the whole header as a mess of macros.

------

Define a couple of `inline` helper functions that do the conversion, like so:

```c
inline uint16_t Reverse16(uint16_t value)
{
    return (((value & 000xFF) << 8) |
            ((value & 0xFF00) >> 8));
}

inline uint32_t Reverse32(uint32_t value) 
{
    return (((value & 0x000000FF) << 24) |
            ((value & 0x0000FF00) <<  8) |
            ((value & 0x00FF0000) >>  8) |
            ((value & 0xFF000000) >> 24));
}
```

- Although it is not strictly necessary, I have kept your explicit masking because I think it increases the clarity of the code, both for human readers and for the compiler. Others may feel that simpler is better. For example, `Reverse16` could simply be implemented as `return ((value << 8) | (value >> 8));`.

- However, I've chosen to format your code slightly differently for nice **vertical alignment**. I think this makes it easier to read, and easier to audit for correctness at a glance.

- On most compilers, there are byte-swapping intrinsics that you could have used to implement the body of these functions. For example, GNU compilers (including GCC and Clang) have `__bswap_32` and `__bswap_16` macros in the `<byteswap.h>` header, and Microsoft's compiler offers the `_byteswap_ushort` and `_byteswap_ulong` intrinsics in the `<intrin.h>` header.

  While intrinsics can often result in better code than writing out the C code long-form, all compilers I tested here are *exceptionally* smart: GCC, Clang, and ICC all recognize the bit-twiddling code used above and compile it to identical object code as if we had used the intrinsic—a single `BSWAP` instruction on the x86 architecture! Microsoft's compiler makes this optimization for the 32-bit version, but not for the 16-bit version. However, its output either way is still perfectly reasonable, and if you're seeking portable code, there is no compelling interest in using the intrinsics. For once, you don't need them to get optimal code!

------

Now that we have those helper functions, we need to define some more inline functions that work like your macros, except that the conditional logic will be encapsulated within the body of the functions, rather than surrounding the macro definitions.

Naturally, since these are functions, not macros, we'll use a different (title-case) naming convention. We don't need SCREAMING_CASE because they're not scary anymore:

```c
inline uint16_t LittleEndian16(uint16_t value)
{
#if __BYTE_ORDER__ == __ORDER_LITTLE_ENDIAN__
    return value;
#elif __BYTE_ORDER__ == __ORDER_BIG_ENDIAN__
    return Reverse16(value);
#else
#    error unsupported endianness
#endif
}

inline uint16_t BigEndian16(uint16_t value)
{
#if __BYTE_ORDER__ == __ORDER_LITTLE_ENDIAN__
    return Reverse16(value);
#elif __BYTE_ORDER__ == __ORDER_BIG_ENDIAN__
    return value;
#else
#    error unsupported endianness
#endif
}

inline uint32_t LittleEndian32(uint32_t value)
{
#if __BYTE_ORDER__ == __ORDER_LITTLE_ENDIAN__
    return value;
#elif __BYTE_ORDER__ == __ORDER_BIG_ENDIAN__
    return Reverse32(value);
#else
#    error unsupported endianness
#endif
}

inline uint32_t BigEndian32(uint32_t value)
{
#if __BYTE_ORDER__ == __ORDER_LITTLE_ENDIAN__
    return Reverse32(value);
#elif __BYTE_ORDER__ == __ORDER_BIG_ENDIAN__
    return value;
#else
#    error unsupported endianness
#endif
}
```

The complete code is slightly longer than the macro-based version, but the safety and other benefits of functions more than justify this expanded length. The compiler doesn't care, and it only takes a few minutes longer to write. You will more than make up for it later in the time you *don't* have to spend debugging macros used in expressions.

You'll use them in basically the same way as the macros. Instead of `LITTLE_ENDIAN_SHORT(value)`, you'd call `LittleEndian16(value)`. Note that I've also used the explicit bit-width in the function's names, instead of the ambiguous `short` and `long` type names.



### stackexchange [Implementation of C Standard Library Function ntohl()](https://codereview.stackexchange.com/questions/149717/implementation-of-c-standard-library-function-ntohl)





### stackoverflow [Convert Little Endian to Big Endian](https://stackoverflow.com/questions/19275955/convert-little-endian-to-big-endian)



## Query 

### Compile time

quarkslab [Unaligned accesses in C/C++: what, why and solutions to do it properly](https://blog.quarkslab.com/unaligned-accesses-in-cc-what-why-and-solutions-to-do-it-properly.html) 中的例子：

```c++
#include <stdint.h>
#include <stdlib.h>

static uint64_t load64_le(uint8_t const* V)
{
#if !defined(__LITTLE_ENDIAN__)
#error This code only works with little endian systems
#endif
  uint64_t Ret = *((uint64_t const*)V);
  return Ret;
}
```



### Dynamic

这个例子是源自creference `Objects and alignment#Strict aliasing`

```C++
#include <cstdio>

int main()
{
	int i = 7;//最低有效位是0x7
	char* pc = (char*) (&i);
	if (pc[0] == '\x7') // aliasing through char is ok
		puts("This system is little-endian");
	else
		puts("This system is big-endian");

}

```

`i`的最低有效位是`0x7`，所以如果低地址`pc[0]`的值等于`'\x7'`，则是little-endian。