# Signedness

1、unsigned：无符号/无正负（类型） signed：有符号/有正负（类型）

2、相同的长度，两者能够表达的范围是相同的




## wikipedia [Signedness](https://en.wikipedia.org/wiki/Signedness)

In computing, **signedness** is a property of [data types](https://en.wikipedia.org/wiki/Data_type) representing [numbers](https://en.wikipedia.org/wiki/Number) in computer programs. A numeric variable is *signed* if it can represent both [positive](https://en.wikipedia.org/wiki/Positive_number) and [negative](https://en.wikipedia.org/wiki/Negative_number) numbers, and *unsigned* if it can only represent [non-negative](https://en.wikipedia.org/wiki/Non-negative) numbers (zero or positive numbers).

As [*signed*](https://en.wikipedia.org/wiki/Sign_(mathematics)) numbers can represent **negative numbers**(负数), they lose a range of **positive numbers** that can only be represented with *unsigned* numbers of the same size (in bits) because half the possible [values](https://en.wikipedia.org/wiki/Value_(programming)) are non-positive values (so if an 8-bit is signed, positive unsigned values 128($2^7$) to 255($2^8 -1$) are gone while -128 to 127 are present). **Unsigned variables** can dedicate all the possible values to the positive number range.

For example, a [two's complement](https://en.wikipedia.org/wiki/Two%27s_complement) signed [16-bit](https://en.wikipedia.org/wiki/16-bit) integer can hold the values −32768 to 32767 inclusively, while an unsigned 16 bit integer can hold the values 0 to [65535](https://en.wikipedia.org/wiki/65535_(number)). For this [sign representation](https://en.wikipedia.org/wiki/Signed_number_representations) method, the leftmost bit ([most significant bit](https://en.wikipedia.org/wiki/Most_significant_bit)) denotes whether the value is positive or negative (0 for positive, 1 for negative).

### In programming languages

For most architectures, there is no signed–unsigned type distinction in the [machine language](https://en.wikipedia.org/wiki/Machine_language). Nevertheless, [arithmetic](https://en.wikipedia.org/wiki/Computer_arithmetic) instructions usually set different [CPU flags](https://en.wikipedia.org/wiki/Status_register) such as the [carry flag](https://en.wikipedia.org/wiki/Carry_flag) for unsigned arithmetic and the [overflow flag](https://en.wikipedia.org/wiki/Overflow_flag) for signed. Those values can be taken into account by subsequent [branch](https://en.wikipedia.org/wiki/Branch_instruction) or arithmetic commands.

The [C programming language](https://en.wikipedia.org/wiki/C_programming_language), along with its derivatives, implements a signedness for all [integer data types](https://en.wikipedia.org/wiki/Integer_(computing)), as well as for ["character"](https://en.wikipedia.org/wiki/Character_(computing)). The `unsigned` modifier defines the type to be unsigned. The default integer signedness is signed, but can be set explicitly with `signed` modifier. Integer [literals](https://en.wikipedia.org/wiki/Literal_(computer_programming)) can be made unsigned with `U` suffix. For example, `0xFFFFFFFF` gives −1, but `0xFFFFFFFFU` gives 4,294,967,295 for 32-bit code.

Compilers often issue a warning when comparisons are made between **signed** and **unsigned** numbers or when one is [cast](https://en.wikipedia.org/wiki/Type_conversion) to the other. These are potentially dangerous operations as the ranges of the signed and unsigned types are different.

### See also

- [Signed number representations](https://en.wikipedia.org/wiki/Signed_number_representations)



## cppreference [Memory model ](https://en.cppreference.com/w/c/language/memory_model)

The [types](https://en.cppreference.com/w/c/language/types) char, unsigned char, and signed char use one byte for both storage and [value representation](https://en.cppreference.com/w/c/language/object). The number of bits in a byte is accessible as [CHAR_BIT](https://en.cppreference.com/w/c/types/limits).

## wikipedia [Signed-number-representations](https://en.wikipedia.org/wiki/Signed_number_representations)

In [computing](https://en.wikipedia.org/wiki/Computing), **signed number representations** are required to encode [negative numbers](https://en.wikipedia.org/wiki/Negative_number) in binary number systems.

In [mathematics](https://en.wikipedia.org/wiki/Mathematics), negative numbers in any base are represented by prefixing them with a minus ("−") sign. However, in [computer hardware](https://en.wikipedia.org/wiki/Computer_hardware), numbers are represented only as sequences of [bits](https://en.wikipedia.org/wiki/Bit), without extra symbols. The four best-known methods of extending the [binary numeral system](https://en.wikipedia.org/wiki/Binary_numeral_system) to represent [signed numbers](https://en.wikipedia.org/wiki/Signed_number) are: [sign-and-magnitude](https://en.wikipedia.org/wiki/Signed_number_representations#Sign-and-magnitude_method), [ones' complement](https://en.wikipedia.org/wiki/Signed_number_representations#Ones'_complement), [two's complement](https://en.wikipedia.org/wiki/Signed_number_representations#Two's_complement), and [offset binary](https://en.wikipedia.org/wiki/Signed_number_representations#Excess-K). Some of the alternative methods use implicit instead of explicit signs, such as negative binary, using the [base −2](https://en.wikipedia.org/wiki/Signed_number_representations#Base_%E2%88%922). Corresponding methods can be devised for [other bases](https://en.wikipedia.org/wiki/Positional_notation), whether positive, negative, fractional, or other elaborations(详细阐述) on such themes.

There is no definitive criterion by which any of the representations is universally superior. The representation used in most current computing devices is **two's complement**, although the [Unisys ClearPath Dorado series](https://en.wikipedia.org/wiki/UNIVAC_1100/2200_series) mainframes use ones' complement.



## stackoverflow [value vs type: Code to Determine if a Variable Is Signed or Not](https://stackoverflow.com/questions/7469915/value-vs-type-code-to-determine-if-a-variable-is-signed-or-not)



I came across this question in a forum. The answer is something like this:

```c
#define ISUNSIGNED(a) (a >= 0 && ~a >= 0) 

//Alternatively, assuming the argument is to be a type, one answer would use type casts: 

#define ISUNSIGNED(type) ((type)0 - 1 > 0)
```

I have a few questions regarding this.Why do we need to check `~a >= 0`? What is the second solution all about? I did not understand the statement: "argument is to be a type". More importantly the author states that first `#define` will not work in ANSI C (but will work in K&R C). Why not?



[A](https://stackoverflow.com/a/7470062)

```c
#define ISUNSIGNED(a) (a >= 0 && ~a >= 0) 
```

For a signed value which is positive, `a >= 0` will be true (obviously) and `~a >= 0` will be false since we've flipped the bits so the sign bit is now set, resulting in a negative value. The entire expression is therefore false.

For a signed value which is negative, `a >= 0` will be false (obviously) and the rest of the expression will not be evaluated; the overall result for the expression is false.

For an unsigned value, `a >= 0` will **always** be true (obviously, since unsigned values can't be negative). If we flip the bits then `~a >= 0` is also true, since even with the most significant bit (the sign bit) set to 1, it's still treated as a positive value.

So, the expression returns true if the original value and its bitwise inverse are both positive, i.e. it's an unsigned value.

```c
#define ISUNSIGNED(type) ((type)0 - 1 > 0)
```

This is to be called with a type rather than a value: `ISUNSIGNED(int)` or `ISUNSIGNED(unsigned int)`, for example.

For an `int`, the code expands to

```c
((int)0 - 1 > 0)  
```

which is false, since `-1` is not greater than `0`.

For an `unsigned int`, the code expands to

```c
((unsigned int)0 - 1 > 0) 
```

The signed `1` and `0` literals in the expression are promoted to `unsigned` to match the first `0`, so the entire expression is evaluated as an unsigned comparison. `0 - 1` in unsigned arithmetic will wrap around resulting in the largest possible unsigned value (all bits set to 1), which is greater than 0, so the result is true.

As to why it would work with K&R C, but not ANSI C, maybe [this article](http://www.informatik.uni-hamburg.de/RZ/software/SUNWspro/c-compiler/transition/tguide.doc.html) can shed some light:

> When an unsigned char or unsigned short is widened, the result type is int if an int is large enough to represent all the values of the smaller type. Otherwise, the result type is unsigned int. The value preserving rule produces the least surprise arithmetic result for most expressions.

I guess that means that when comparing an `unsigned short` to `0`, for example, the unsigned value is converted to a `signed int` which breaks the behaviour of the macro.

You can probably work around this by having `(a-a)` which evaluates to either signed or unsigned zero as appropriate, instead of the literal `0` which is always signed.