# Float point number

1、在 `Book-计算机组成原理-科学出版社-2-运算方法与运算器` 中，对Float point number的存储格式进行了非常好的分析，本文不再进行原理的深入讨论

2、trade off: range 和 precision，显然floating point number是牺牲precision来换取range

在 cnblogs [从如何判断浮点数是否等于0说起——浮点数的机器级表示](https://www.cnblogs.com/kubixuesheng/p/4107309.html) 中，也有这样的论述

## wikipedia [Computer number format](https://en.wikipedia.org/wiki/Computer_number_format) # Floating-point numbers

> NOTE: 
>
> 1、这种方式目前是主流

While both **unsigned** and **signed** integers are used in digital systems, even a 32-bit integer is not enough to handle all the range of numbers a calculator can handle, and that's not even including fractions. To approximate the greater range and precision of [real numbers](https://en.wikipedia.org/wiki/Real_number), we have to abandon signed integers and fixed-point numbers and go to a "[floating-point](https://en.wikipedia.org/wiki/Floating_point)" format.

> NOTE: 
>
> 1、这段话道出了使用floating pointer number的原因: 使用相同位数的bit，能够表示更大的范围("range ")

In the decimal system, we are familiar with floating-point numbers of the form ([scientific notation](https://en.wikipedia.org/wiki/Scientific_notation)):

> NOTE: 
>
> 1、" [scientific notation](https://en.wikipedia.org/wiki/Scientific_notation) " 即 "科学计数法"

$1.1030402 × 10^{5} = 1.1030402 × 100000 = 110304.02$

or, more compactly:

> NOTE: 
>
> 1、"compactly"的含义是"紧密"

`1.1030402E5`



which means "1.1030402 times 1 followed by 5 zeroes". We have a certain numeric value (`1.1030402`) known as a "[significand](https://en.wikipedia.org/wiki/Significand)", multiplied by a power of 10 (E5, meaning $10^5$ or `100,000`), known as an "[exponent](https://en.wikipedia.org/wiki/Exponentiation)". If we have a negative exponent, that means the number is multiplied by a 1 that many places to the right of the decimal point. For example:

> NOTE: 
>
> 1、"Significand"的含义是"有效数字"

$2.3434E−6 = 2.3434 × 10^{−6} = 2.3434 × 0.000001 = 0.0000023434$

The advantage of this scheme is that by using the **exponent** we can get a much wider **range** of numbers, even if the number of digits in the significand, or the "numeric precision", is much smaller than the range. 

> NOTE: 
>
> 1、trade off: range 和 precision，显然floating point number是牺牲precision来换取range

Similar binary floating-point formats can be defined for computers. There is a number of such schemes, the most popular has been defined by [Institute of Electrical and Electronics Engineers](https://en.wikipedia.org/wiki/Institute_of_Electrical_and_Electronics_Engineers) (IEEE). The [IEEE 754-2008](https://en.wikipedia.org/wiki/IEEE_floating_point) standard specification defines a 64 bit floating-point format with:

- an 11-bit binary exponent, using "excess-1023" format. Excess-1023 means the exponent appears as an unsigned binary integer from 0 to 2047; subtracting 1023 gives the actual signed value
- a 52-bit significand, also an unsigned binary number, defining a fractional value with a leading implied "1"
- a sign bit, giving the sign of the number.

Let's see what this format looks like by showing how such a number would be stored in 8 bytes of memory:



## wikipedia [Floating-point arithmetic](https://en.wikipedia.org/wiki/Floating-point_arithmetic)

In [computing](https://en.wikipedia.org/wiki/Computing), **floating-point arithmetic** (**FP**) is arithmetic using formulaic representation of [real numbers](https://en.wikipedia.org/wiki/Real_number) as an **approximation** so as to support a [trade-off](https://en.wikipedia.org/wiki/Trade-off) between range and [precision](https://en.wikipedia.org/wiki/Accuracy_and_precision). For this reason, floating-point computation is often found in systems which include very small and very large [real numbers](https://en.wikipedia.org/wiki/Real_numbers), which require fast processing times. A number is, in general, represented approximately to a fixed number of [significant digits](https://en.wikipedia.org/wiki/Significant_figures) (the [significand](https://en.wikipedia.org/wiki/Significand)) and scaled using an [exponent](https://en.wikipedia.org/wiki/Exponentiation) in some fixed **base**; the base for the scaling is normally two, ten, or sixteen. A number that can be represented exactly is of the following form:

![{\text{significand}}\times {\text{base}}^{\text{exponent}},](https://wikimedia.org/api/rest_v1/media/math/render/svg/1d3df0e2c38ef77dd2cd42114520079bd76b6670)

> NOTE: 
>
> 1、其实这就是科学计数法
>
> 2、[significand](https://en.wikipedia.org/wiki/Significand)的中文意思就是：有效数，或者是尾数；在二进制计算机中，**base**就是2；
>
> 

where significand is an [integer](https://en.wikipedia.org/wiki/Integer), base is an integer greater than or equal to two, and exponent is also an integer. For example:

![1.2345=\underbrace {12345} _{\text{significand}}\times \underbrace {10} _{\text{base}}\!\!\!\!\!\!^{\overbrace {-4} ^{\text{exponent}}}.](https://wikimedia.org/api/rest_v1/media/math/render/svg/ae814346939ac31086e1d0286c41d98e6b053102)

The term *floating point* refers to the fact that a number's [radix point](https://en.wikipedia.org/wiki/Radix_point) (*decimal point*, or, more commonly in computers, *binary point*) can "float"; that is, it can be placed anywhere relative to the [significant digits](https://en.wikipedia.org/wiki/Significant_digits) of the number. This position is indicated as the exponent component, and thus the floating-point representation can be thought of as a kind of [scientific notation](https://en.wikipedia.org/wiki/Scientific_notation).

### Accuracy problems

> NOTE: 
>
> 1、本节讨论的是精度问题，这是非常重要的，这对于理解后面的`Comparison`章节的内容非常重要
>
> 2、这段话让我想起了中学时的一个问题: 数轴是否存在能够表示一个小数部分有无限多位的数，比如π，答案是可以的；
>
> 但是计算机肯定是不可以的，因为浮点数的长度是有限的，这就限制了它的精度。

The fact that floating-point numbers cannot precisely represent all real numbers, and that floating-point operations cannot precisely represent true arithmetic operations, leads to many surprising situations. This is related to the finite [precision](https://en.wikipedia.org/wiki/Precision_(computer_science)) with which computers generally represent numbers.

> NOTE: 
>
> 1、这句话也道出了computer的局限性

For example, the **non-representability** of 0.1 and 0.01 (in binary) means that the result of attempting to square(平方运算) 0.1 is neither 0.01 nor the representable number closest to it. In 24-bit (single precision) representation, 0.1 (decimal) was given previously as *e* = −4; *s* = 110011001100110011001101, which is

`0.100000001490116119384765625` exactly.

Squaring this number gives

`0.010000000298023226097399174250313080847263336181640625` exactly.

> NOTE: 
>
> 1、上面这段话初读起来是违反直觉的
>
> 2、后面的很多例子也是非常违反直觉的

Also, the **non-representability** of π (and π/2) means that an attempted computation of tan(π/2) will not yield a result of infinity, nor will it even overflow in the usual floating-point formats (assuming an accurate implementation of tan). It is simply not possible for standard floating-point hardware to attempt to compute tan(π/2), because π/2 cannot be represented exactly. This computation in C:

```
/* Enough digits to be sure we get the correct approximation. */
double pi = 3.1415926535897932384626433832795;
double z = tan(pi/2.0);
```

will give a result of 16331239353195370.0. In single precision (using the tanf function), the result will be −22877332.0.

In addition to loss of significance, inability to represent numbers such as π and 0.1 exactly, and other slight inaccuracies, the following phenomena may occur:

1、[Cancellation](https://en.wikipedia.org/wiki/Catastrophic_cancellation): 

> NOTE: 
>
> 1、没有深入阅读

2、Conversions to integer are not intuitive: converting (63.0/9.0) to integer yields 7, but converting (0.63/0.09) may yield 6. This is because conversions generally truncate rather than round. [Floor and ceiling functions](https://en.wikipedia.org/wiki/Floor_and_ceiling_functions) may produce answers which are off by one from the intuitively expected value.

> NOTE: "conversions generally truncate rather than round"的意思是: conversion通常是截断而不是四舍五入
>
> 下面是测试程序:
>
> ```C++
> #include <iostream>
> 
> int main()
> {
> 	int i = (63.0 / 9.0);
> 	std::cout << i << std::endl;
> 	i = (0.63 / 0.09);
> 	std::cout << i << std::endl;
> }
> // g++ test.cpp
> 
> ```

3、Limited exponent range: results might overflow yielding infinity, or underflow yielding a [subnormal number](https://en.wikipedia.org/wiki/Subnormal_number) or zero. In these cases precision will be lost.

> NOTE: 
>
> 1、这段话其实讨论的是overflow 和 underflow

4、Testing for [safe division](https://en.wikipedia.org/wiki/Division_by_zero#Computer_arithmetic) is problematic: Checking that the divisor is not zero does not guarantee that a division will not overflow.

5、Testing for equality is problematic. Two computational sequences that are mathematically equal may well produce different floating-point values.

> NOTE: 
>
> 1、"Testing for equality"是需要一些技巧的，这在 `Comparison` 章节中进行了讨论、

#### Incidents



#### Machine precision and backward error analysis



#### Minimizing the effect of accuracy problems



## wikipedia [Single-precision floating-point format](https://en.wikipedia.org/wiki/Single-precision_floating-point_format)

**Single-precision floating-point format** is a [computer number format](https://en.wikipedia.org/wiki/Computer_number_format), usually occupying [32 bits](https://en.wikipedia.org/wiki/32_bits) in [computer memory](https://en.wikipedia.org/wiki/Computer_memory); it represents a wide [dynamic range](https://en.wikipedia.org/wiki/Dynamic_range) of numeric values by using a [floating radix point](https://en.wikipedia.org/wiki/Floating_point).

A floating-point variable can represent a wider range of numbers than a [fixed-point](https://en.wikipedia.org/wiki/Fixed-point_arithmetic) variable of the same bit width at the cost of precision. A [signed](https://en.wikipedia.org/wiki/Signedness)32-bit [integer](https://en.wikipedia.org/wiki/Integer) variable has a maximum value of $2^{31} − 1 = 2,147,483,647$, whereas an IEEE 754 32-bit base-2 floating-point variable has a maximum value of $(2 − 2^{−23}) × 2^{127} ≈ 3.4028235 × 10^{38}$. All integers with 6 or fewer [significant decimal digits](https://en.wikipedia.org/wiki/Significant_figures), and any number that can be written as 2n such that n is a whole number from -126 to 127, can be converted into an IEEE 754 floating-point value without loss of precision.

In the [IEEE 754-2008](https://en.wikipedia.org/wiki/IEEE_754-2008) [standard](https://en.wikipedia.org/wiki/Standardization), the 32-bit base-2 format is officially referred to as **binary32**; it was called **single** in [IEEE 754-1985](https://en.wikipedia.org/wiki/IEEE_754-1985). IEEE 754 specifies additional floating-point types, such as 64-bit base-2 *double precision* and, more recently, base-10 representations.

One of the first [programming languages](https://en.wikipedia.org/wiki/Programming_language) to provide single- and double-precision floating-point data types was [Fortran](https://en.wikipedia.org/wiki/Fortran). Before the widespread adoption of IEEE 754-1985, the representation and properties of floating-point data types depended on the [computer manufacturer](https://en.wikipedia.org/wiki/Computer_manufacturer) and computer model, and upon decisions made by programming-language designers. E.g., [GW-BASIC](https://en.wikipedia.org/wiki/GW-BASIC)'s single-precision data type was the [32-bit MBF](https://en.wikipedia.org/wiki/32-bit_MBF) floating-point format.

Single precision is termed *REAL* in [Fortran](https://en.wikipedia.org/wiki/Fortran),[[1\]](https://en.wikipedia.org/wiki/Single-precision_floating-point_format#cite_note-1) *SINGLE-FLOAT* in [Common Lisp](https://en.wikipedia.org/wiki/Common_Lisp),[[2\]](https://en.wikipedia.org/wiki/Single-precision_floating-point_format#cite_note-2) *float* in [C](https://en.wikipedia.org/wiki/C_(programming_language)), [C++](https://en.wikipedia.org/wiki/C%2B%2B), [C#](https://en.wikipedia.org/wiki/C_Sharp_(programming_language)), [Java](https://en.wikipedia.org/wiki/Java_(programming_language)),[[3\]](https://en.wikipedia.org/wiki/Single-precision_floating-point_format#cite_note-3) *Float* in [Haskell](https://en.wikipedia.org/wiki/Haskell_(programming_language)),[[4\]](https://en.wikipedia.org/wiki/Single-precision_floating-point_format#cite_note-4) and *Single* in [Object Pascal](https://en.wikipedia.org/wiki/Object_Pascal) ([Delphi](https://en.wikipedia.org/wiki/Delphi_(programming_language))), [Visual Basic](https://en.wikipedia.org/wiki/Visual_Basic), and [MATLAB](https://en.wikipedia.org/wiki/MATLAB). However, *float* in [Python](https://en.wikipedia.org/wiki/Python_(programming_language)), [Ruby](https://en.wikipedia.org/wiki/Ruby_(programming_language)), [PHP](https://en.wikipedia.org/wiki/PHP), and [OCaml](https://en.wikipedia.org/wiki/OCaml) and *single* in versions of [Octave](https://en.wikipedia.org/wiki/GNU_Octave) before 3.2 refer to [double-precision](https://en.wikipedia.org/wiki/Double-precision_floating-point_format) numbers. In most implementations of [PostScript](https://en.wikipedia.org/wiki/PostScript), and some [embedded systems](https://en.wikipedia.org/wiki/Embedded_systems), the only supported precision is single.



## TODO

zhihu [为什么叫浮点数?](https://www.zhihu.com/question/19848808)