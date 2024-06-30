# Integer overflow

一、当发生overflow的时候，有两种处理方式:

1、*wrapping*

wrap around

2、*saturating* 

二、思考: why not throw exception when integer overflow in c++



## wikipedia [Integer overflow](https://en.wikipedia.org/wiki/Integer_overflow)

In [computer programming](https://en.wikipedia.org/wiki/Computer_programming), an **integer overflow** occurs when an [arithmetic](https://en.wikipedia.org/wiki/Arithmetic) operation attempts to create a numeric value that is outside of the range that can be represented with a given number of digits – either larger than the maximum or lower than the minimum representable value.

> NOTE: 
>
> 1、tag-non-representability-representable-out of range

The most common result of an overflow is that the **least significant representable digits** of the result are stored; the result is said to *wrap* around the **maximum** (i.e. [modulo](https://en.wikipedia.org/wiki/Modular_arithmetic) a power of the [radix](https://en.wikipedia.org/wiki/Radix), usually two in modern computers, but sometimes ten or another radix).

> NOTE: 
>
> 1、在原文中给出了一个 [odometer](https://en.wikipedia.org/wiki/Odometer) 在发生overflow时wrap around的例子
>
> 2、上面这段话，需要结合具体的例子来进行理解，其实非常简答: 
>
> ```C++
> #include <iostream>
> #include <limits>
> 
> int main()
> {
> 	unsigned short int a = std::numeric_limits<unsigned short int>::max() + 1;
> 	std::cout << a << std::endl;
> 	return 0;
> }
> // g++ test.cpp
> 
> ```
>
> 编译警告如下:
>
> ```c++
> test.cpp: In function 'int main()':
> test.cpp:6:72: warning: unsigned conversion from 'int' to 'short unsigned int' changes value from '65536' to '0' [-Woverflow]
>   unsigned short int a = std::numeric_limits<unsigned short int>::max() + 1;
> 
> ```
>
> 输出如下:
>
> ```C++
> 0
> ```
>
> 当溢出后，会将进位给抛弃。

An **overflow condition** may give results leading to unintended behavior. In particular, if the possibility has not been anticipated(预料), overflow can compromise a program's reliability and [security](https://en.wikipedia.org/wiki/Software_security).

For some applications, such as **timers** and **clocks**, wrapping on overflow can be desirable. The [C11](https://en.wikipedia.org/wiki/C_programming_language) standard states that for **unsigned integers** modulo wrapping is the **defined behavior** and the term overflow never applies: "a computation involving unsigned operands can never overflow."[[1\]](https://en.wikipedia.org/wiki/Integer_overflow#cite_note-auto-1)

On some processors like [graphics processing units](https://en.wikipedia.org/wiki/Graphics_processing_unit) (GPUs) and [digital signal processors](https://en.wikipedia.org/wiki/Digital_signal_processor) (DSPs) which support [saturation arithmetic](https://en.wikipedia.org/wiki/Saturation_arithmetic), overflowed results would be "clamped", i.e. set to the minimum or the maximum value in the representable range, rather than **wrapped around**.

> NOTE: 
>
> 一、 [wikipedia Saturation arithmetic](https://en.wikipedia.org/wiki/Saturation_arithmetic) 中的内容可以帮助读懂上面这段话，下面是其中的一个例子:
>
> > 60 + 43 → 100. (*not* the expected 103.)
>
> "clamped"的表面意思是"夹住"，结合[wikipedia Saturation arithmetic](https://en.wikipedia.org/wiki/Saturation_arithmetic) 中的内容可知，它会被设置为可表示范围内的最小值或最大值，而不是缠绕

### Origin



In particular, multiplying or adding two integers may result in a value that is unexpectedly small, and subtracting from a small integer may cause a wrap to a large positive value (for example, 8-bit integer addition 255 + 2 results in 1, which is 257 mod $2^{8}$, and similarly subtraction 0 − 1 results in 255, a [two's complement](https://en.wikipedia.org/wiki/Two's_complement) representation of −1).

> NOTE: 
>
> 1、"8-bit integer addition 255 + 2 " example: 
>
> ```C++
> #include <iostream>
> #include <limits>
> #include <cstdint>
> 
> int main()
> {
> 	uint8_t a = 255;
> 	uint8_t b = 2;
> 	uint8_t c = a + b;
> 	std::cout << c << std::endl;
> 	return 0;
> }
> // g++ test.cpp --std=c++11
> 
> ```
>
> 实际输出非常奇怪，输出为空
>
> 2、"0 − 1 results in 255" example
>
> ```C++
> #include <iostream>
> #include <limits>
> #include <cstdint>
> 
> int main()
> {
> 	uint16_t a = 0 - 1;
> 	std::cout << a << std::endl;
> 	return 0;
> }
> // g++ test.cpp --std=c++11
> 
> ```
>
> 

#### unsigned integer overflow + wraparound 导致 buffer overflow 

Such wraparound may cause security detriments—if an overflowed value is used as the number of bytes to allocate for a buffer, the buffer will be allocated unexpectedly small, potentially leading to a buffer overflow which, depending on the usage of the buffer, might in turn cause arbitrary code execution.

#### signed integer overflow + wraparound

If the variable has a [signed integer](https://en.wikipedia.org/wiki/Signed_number_representations) type, a program may make the assumption that a variable always contains a positive value. An integer overflow can cause the value to wrap and become negative, which violates the program's assumption and may lead to unexpected behavior (for example, 8-bit integer addition of 127 + 1 results in −128, a two's complement of 128). (A solution for this particular problem is to use unsigned integer types for values that a program expects and assumes will never be negative.)

> NOTE: 
>
> 1、测试程序如下:
>
> ```C++
> #include <iostream>
> #include <limits>
> 
> int main()
> {
> 	int a = std::numeric_limits<int>::max() + 1;
> 	std::cout << a << std::endl;
> 	std::cout << std::numeric_limits<int>::min() << std::endl;
> 	return 0;
> }
> // g++ test.cpp 
> 
> ```
>
> 输出如下:
>
> ```C++
> -2147483648
> -2147483648
> ```
>
> 2、为什么结果是`std::numeric_limits<int>::min()`？
>
> 可以从如下两个方面来进行分析:
>
> a、wrap around: signed number的wrap around显然不是回到了0，而是从`min`开始
>
> b、从底层bit来进行分析，下面以8 bit int来简要说明:
>
> ```C++
> max: 01111111
> max + 1: 10000000 (进位)
> min: 10000000 (二进制补码的最大值)   
> ```



If the variable has a [signed integer](https://en.wikipedia.org/wiki/Signed_number_representations) type, a program may make the assumption that a variable always contains a positive value. An integer overflow can cause the value to wrap and become negative, which violates the program's assumption and may lead to unexpected behavior (for example, 8-bit integer addition of 127 + 1 results in −128, a two's complement of 128). (A solution for this particular problem is to use unsigned integer types for values that a program expects and assumes will never be negative.)

> NOTE: 
>
> 1、这和cppcoreguidelines [ES.106: Don't try to avoid negative values by using `unsigned`](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md#es106-dont-try-to-avoid-negative-values-by-using-unsigned) 相违背



### Flags

Most computers have two dedicated processor flags to check for overflow conditions.

### Inconsistent behavior

In C, unsigned integer overflow is defined to wrap around, while signed integer overflow causes [undefined behavior](https://en.wikipedia.org/wiki/Undefined_behavior).



### Methods to address integer overflow problems

|                           Language                           |                       Unsigned integer                       |                        Signed integer                        |
| :----------------------------------------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
| [Ada](https://en.wikipedia.org/wiki/Ada_(programming_language)) |                  modulo the type's modulus                   |                   `raise Constraint_Error`                   |
| [C](https://en.wikipedia.org/wiki/C_(programming_language))/[C++](https://en.wikipedia.org/wiki/C%2B%2B) |                     modulo power of two                      |                      undefined behavior                      |
| [C#](https://en.wikipedia.org/wiki/C_Sharp_(programming_language)) | modulo power of 2 in unchecked context; `System.OverflowException` is raised in checked context[[11\]](https://en.wikipedia.org/wiki/Integer_overflow#cite_note-11) |                                                              |
| [Java](https://en.wikipedia.org/wiki/Java_(programming_language)) |                             N/A                              |                     modulo power of two                      |
|    [JavaScript](https://en.wikipedia.org/wiki/JavaScript)    | all numbers are [double-precision floating-point](https://en.wikipedia.org/wiki/Double-precision_floating-point_format) except the new [BigInt](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/BigInt) |                                                              |
|        [MATLAB](https://en.wikipedia.org/wiki/MATLAB)        | Builtin integers saturate. Fixed-point integers configurable to wrap or saturate |                                                              |
| [Python](https://en.wikipedia.org/wiki/Python_(programming_language)) 2 |                             N/A                              |               convert to `long` type (bigint)                |
|         [Seed7](https://en.wikipedia.org/wiki/Seed7)         |                             N/A                              | `**raise** OVERFLOW_ERROR`[[12\]](https://en.wikipedia.org/wiki/Integer_overflow#cite_note-12) |
| [Scheme](https://en.wikipedia.org/wiki/Scheme_(programming_language)) |                             N/A                              |                      convert to bigNum                       |
|      [Simulink](https://en.wikipedia.org/wiki/Simulink)      |               configurable to wrap or saturate               |                                                              |
|     [Smalltalk](https://en.wikipedia.org/wiki/Smalltalk)     |                             N/A                              |                  convert to `LargeInteger`                   |
| [Swift](https://en.wikipedia.org/wiki/Swift_(programming_language)) | Causes error unless using special overflow operators.[[13\]](https://en.wikipedia.org/wiki/Integer_overflow#cite_note-13) |                                                              |





#### Detection

> NOTE:
>
> 1、通过tool来进行detection

Run-time overflow detection implementation `UBSan` is available for [C compilers](https://en.wikipedia.org/wiki/C_compiler).

In Java 8, there are [overloaded methods](https://en.wikipedia.org/wiki/Function_overloading), for example like `Math.addExact(int, int)`, which will throw `ArithmeticException` in case of overflow.

> NOTE:
>
> 1、及时抛出exception是非常好的，符合"error handling-report error on time not run on error-及时报告错误-而不是在错误的基础上运行"原则

[Computer emergency response team](https://en.wikipedia.org/wiki/Computer_emergency_response_team) (CERT) developed the As-if Infinitely Ranged (AIR) integer model, a largely automated mechanism to eliminate integer overflow and truncation in C/C++ using run-time error handling.[[14\]](https://en.wikipedia.org/wiki/Integer_overflow#cite_note-14)

> NOTE: 
>
> 1、这是在语言层面系统的解决这个问题，但是可以 预期的是，这会带来runtime overhead

#### Avoidance

> NOTE: 
>
> 1、由programmer主动来进行避免

By allocating variables with data types that are large enough to contain all values that may possibly be computed and stored in them, it is always possible to avoid overflow.

> NOTE: 
>
> 1、在实际的工程中，我们往往是按照这个策略的，但是这种方法显然会导致比较麻烦
>
> 2、这种方式其实是由programmer自己来进行保证

Even when the available space or the fixed data types provided by a programming language or environment are too limited to allow for variables to be defensively allocated with generous sizes, by carefully ordering operations and checking operands in advance, it is often possible to ensure *a priori* (先验)that the result will never be larger than can be stored. [Static analysis](https://en.wikipedia.org/wiki/Static_program_analysis) tools, [formal verification](https://en.wikipedia.org/wiki/Formal_verification) and [design by contract](https://en.wikipedia.org/wiki/Design_by_contract) techniques can be used to more confidently and robustly ensure that an overflow cannot accidentally result.



> NOTE:
>
> 1、stackoverflow [How do I detect unsigned integer multiply overflow?](https://stackoverflow.com/questions/199333/how-do-i-detect-unsigned-integer-multiply-overflow) # [A](https://stackoverflow.com/a/1514309) 中就演示了如何来实现"Avoidance"
>
> 

#### Handling

> NOTE: 
>
> 1、这一段的观点和前面的"Avoidance"相同，它更加着重强调的是handling

#### Explicit propagation

> NOTE:
>
> 1、通过一个flag来标识是否发生了overflow，这种方式，需要programmer进行check，显然这在一定程度上也增加了programmer的负担；如果能够让这个标志后续不可用，那么这种方式是可以接受的

if a value is too large to be stored it can be assigned a special value indicating that overflow has occurred and then have all successive operation return this flag value. Such values are sometimes referred to as [NaN](https://en.wikipedia.org/wiki/NaN), for "not a number". This is useful so that the problem can be checked for once at the end of a long calculation rather than after each step. This is often supported in floating point hardware called [FPUs](https://en.wikipedia.org/wiki/Floating_point_unit).



## Detection

一、"运算完成后进行检查，如果发生了overflow，则及时报告错误"这种思路是错误的，由于已经发生了overflow undefined behavior，所以后面的detection是没有意义的，这是在stackoverflow [How do I detect unsigned integer multiply overflow?](https://stackoverflow.com/questions/199333/how-do-i-detect-unsigned-integer-multiply-overflow) # [A](https://stackoverflow.com/a/1514309) 中提出的

二、可行的方法:

1、UBSan

2、prediction

3、使用更宽的类型

4、compiler report error

### UBSan

关于 `UBSan`，参见:

clang [UndefinedBehaviorSanitizer](https://clang.llvm.org/docs/UndefinedBehaviorSanitizer.html)

stackoverflow [gcc-4.9 Undefined Behavior Sanitizer](https://stackoverflow.com/questions/20738232/gcc-4-9-undefined-behavior-sanitizer)

这种方式是非常可行的



### Prediction

相关内容:

1、[LeetCode-7. 整数反转-中等](https://leetcode.cn/problems/reverse-integer/) 

#### stackoverflow [How do I detect unsigned integer multiply overflow?](https://stackoverflow.com/questions/199333/how-do-i-detect-unsigned-integer-multiply-overflow) 

I was writing a program in C++ to find all solutions of $a^b = c$, where *a*, *b* and *c* together use all the digits 0-9 exactly once. The program looped over values of *a* and *b*, and it ran a digit-counting routine each time on *a*, *b* and *ab* to check if the digits condition was satisfied.

However, spurious solutions can be generated when $a^b$ overflows the integer limit. I ended up checking for this using code like:

```c++
unsigned long b, c, c_test;
...
c_test=c*b;         // Possible overflow
if (c_test/b != c) {/* There has been an overflow*/}
else c=c_test;      // No overflow
```

> NOTE:
>
> 一、由于已经发生了overflow undefined behavior，所以后面的detection是没有意义的，这是在 [A](https://stackoverflow.com/a/1514309) 中提出的

Is there a better way of testing for overflow? I know that some chips have an internal flag that is set when overflow occurs, but I've never seen it accessed through C or C++.

------

Beware that ***signed* `int` overflow is undefined behaviour in C and C++**, and thus you have to detect it without actually causing it. For signed int overflow before addition, see *[Detecting signed overflow in C/C++](https://stackoverflow.com/questions/3944505/detecting-signed-overflow-in-c-c)*.

> NOTE:
>
> 一、prediction

#### Comment

一、Information which may be useful on this subject: 

1、Chapter 5 of "Secure Coding in C and C++" by Seacord - http://www.informit.com/content/images/0321335724/samplechapter/seacord_ch05.pdf 

2、`SafeInt` classes for C++ - http://blogs.msdn.com/david_leblanc/archive/2008/09/30/safeint-3-on-codeplex.aspx - http://www.codeplex.com/SafeInt IntSafe library for C: - [[blogs.msdn.com/michael_howard/archiv](http://blogs.msdn.com/michael_howard/archiv) – [Michael Burr](https://stackoverflow.com/users/12711/michael-burr) [Oct 13 '08 at 23:28](https://stackoverflow.com/questions/199333/how-do-i-detect-unsigned-integer-multiply-overflow#comment23412249_199333)



二、Seacord's Secure Coding is a great resource, but don't use IntegerLib. See [blog.regehr.org/archives/593](http://blog.regehr.org/archives/593/). – [jww](https://stackoverflow.com/users/608639/jww) [Sep 26 '11 at 0:53](https://stackoverflow.com/questions/199333/how-do-i-detect-unsigned-integer-multiply-overflow#comment23412250_199333)



三、The gcc compiler option `-ftrapv` will cause it to generate a SIGABRT on (signed) integer overflow. See [here](http://stackoverflow.com/a/5005792/462335). – [nibot](https://stackoverflow.com/users/462335/nibot) [Oct 17 '12 at 20:12](https://stackoverflow.com/questions/199333/how-do-i-detect-unsigned-integer-multiply-overflow#comment17542627_199333)

> NOTE: 
>
> 1、下面收录了这篇文章，非常好

四、It does not answer the overflow question, but another way to come at the problem would be to use a BigNum library like [GMP](http://gmplib.org/) to guarantee you always have enough precision. 





##### [A](https://stackoverflow.com/a/1514309) 

I see you're using unsigned integers. By definition, **in C** (I don't know about C++), unsigned arithmetic does not overflow ... so, at least for C, your point is moot :)

With signed integers, once there has been overflow, [undefined behaviour](http://en.wikipedia.org/wiki/Undefined_behavior) (UB) has occurred and your program can do anything (for example: render tests inconclusive). 

```c++
#include <limits.h>

int a = <something>;
int x = <something>;
a += x;              /* UB */
if (a < 0) {         /* Unreliable test */
  /* ... */
}
```

To create a conforming program, you need to test for overflow **before** generating said overflow. The method can be used with unsigned integers too:

```C++
// For addition
#include <limits.h>

int a = <something>;
int x = <something>;
if ((x > 0) && (a > INT_MAX - x)) /* `a + x` would overflow */;
if ((x < 0) && (a < INT_MIN - x)) /* `a + x` would underflow */;
```

------

```C++
// For subtraction
#include <limits.h>
int a = <something>;
int x = <something>;
if ((x < 0) && (a > INT_MAX + x)) /* `a - x` would overflow */;
if ((x > 0) && (a < INT_MIN + x)) /* `a - x` would underflow */;
```

------

```C++
// For multiplication
#include <limits.h>

int a = <something>;
int x = <something>;
// There may be a need to check for -1 for two's complement machines.
// If one number is -1 and another is INT_MIN, multiplying them we get abs(INT_MIN) which is 1 higher than INT_MAX
if ((a == -1) && (x == INT_MIN)) /* `a * x` can overflow */
if ((x == -1) && (a == INT_MIN)) /* `a * x` (or `a / x`) can overflow */
// general case
if (a > INT_MAX / x) /* `a * x` would overflow */;
if ((a < INT_MIN / x)) /* `a * x` would underflow */;
```

------

For division (except for the `INT_MIN` and `-1` special case), there isn't any possibility of going over `INT_MIN` or `INT_MAX`.



##### [A](https://stackoverflow.com/a/20956705)

[Clang 3.4+](http://clang.llvm.org/docs/LanguageExtensions.html#checked-arithmetic-builtins) and [GCC 5+](https://gcc.gnu.org/gcc-5/changes.html#c-family) offer checked arithmetic builtins. They offer a very fast solution to this problem, especially when compared to bit-testing safety checks.

For the example in OP's question, it would work like this:

```c++
unsigned long b, c, c_test;
if (__builtin_umull_overflow(b, c, &c_test))
{
    // Returned non-zero: there has been an overflow
}
else
{
    // Return zero: there hasn't been an overflow
}
```

The Clang documentation doesn't specify whether `c_test` contains the overflowed result if an overflow occurred, but the GCC documentation says that it does. Given that these two like to be `__builtin`-compatible, it's probably safe to assume that this is how Clang works too.

There is a `__builtin` for each arithmetic operation that can overflow (addition, subtraction, multiplication), with signed and unsigned variants, for int sizes, long sizes, and long long sizes. The syntax for the name is `__builtin_[us](operation)(l?l?)_overflow`:

- `u` for *unsigned* or `s` for *signed*;
- operation is one of `add`, `sub` or `mul`;
- no `l` suffix means that the operands are `int`s; one `l` means `long`; two `l`s mean `long long`.

So for a checked signed long integer addition, it would be `__builtin_saddl_overflow`. The full list can be found on the [Clang documentation page](http://clang.llvm.org/docs/LanguageExtensions.html#checked-arithmetic-builtins).

GCC 5+ and Clang 3.8+ additionally offer generic builtins that work without specifying the type of the values: `__builtin_add_overflow`, `__builtin_sub_overflow` and `__builtin_mul_overflow`. These also work on types smaller than `int`.

The builtins lower to what's best for the platform. On x86, they check the carry, overflow and sign flags.

[Visual Studio](http://en.wikipedia.org/wiki/Microsoft_Visual_Studio)'s cl.exe doesn't have direct equivalents. For unsigned additions and subtractions, including `<intrin.h>` will allow you to use `addcarry_uNN` and `subborrow_uNN` (where NN is the number of bits, like `addcarry_u8` or `subborrow_u64`). Their signature is a bit obtuse:

```c++
unsigned char _addcarry_u32(unsigned char c_in, unsigned int src1, unsigned int src2, unsigned int *sum);
unsigned char _subborrow_u32(unsigned char b_in, unsigned int src1, unsigned int src2, unsigned int *diff);
```

`c_in`/`b_in` is the carry/borrow flag on input, and the return value is the carry/borrow on output. It does not appear to have equivalents for signed operations or multiplications.

Otherwise, Clang for Windows is now production-ready (good enough for Chrome), so that could be an option, too.

##### [A](https://stackoverflow.com/a/199455)

> NOTE: 
>
> 未读



#### stackoverflow [Detecting signed overflow in C/C++](https://stackoverflow.com/questions/3944505/detecting-signed-overflow-in-c-c)

> NOTE:
>
> 一、这是在 stackoverflow [How do I detect unsigned integer multiply overflow?](https://stackoverflow.com/questions/199333/how-do-i-detect-unsigned-integer-multiply-overflow) 中发现的



### Compiler report overflow 

#### stackoverflow [C: avoiding overflows when working with big numbers](https://stackoverflow.com/questions/5005379/c-avoiding-overflows-when-working-with-big-numbers)



##### [A](https://stackoverflow.com/a/5005792)



```C++
/* compile with gcc -ftrapv <filename> */
#include <signal.h>
#include <stdio.h>
#include <limits.h>

void signalHandler(int sig)
{
	printf("Overflow detected\n");
}

int main()
{
	signal(SIGABRT, &signalHandler);

	int largeInt = INT_MAX;
	int normalInt = 42;
	int overflowInt = largeInt + normalInt; /* should cause overflow */

	/* if compiling with -ftrapv, we shouldn't get here */
	return 0;
}
// gcc test.cpp -ftrapv

```

> NOTE: 
>
> 1、在我的机器上并没有生效

#### stackoverflow [Integer overflow in C: standards and compilers](https://stackoverflow.com/questions/3679047/integer-overflow-in-c-standards-and-compilers)



##### [A](https://stackoverflow.com/a/3679149)

Take a look at [`-ftrapv` and `-fwrapv`](http://gcc.gnu.org/onlinedocs/gcc-4.0.2/gcc/Code-Gen-Options.html):

> -ftrapv
>
> This option generates traps for signed overflow on addition, subtraction, multiplication operations.
>
> -fwrapv
>
> This option instructs the compiler to assume that signed arithmetic overflow of addition, subtraction and multiplication wraps around using twos-complement representation. This flag enables some optimizations and disables other. This option is enabled by default for the Java front-end, as required by the Java language specification.



## Example: CSDN [C/C++: 整数溢出(Integer Overflow)](C/C++: 整数溢出(Integer Overflow))

```C++

#include <stdio.h>
 
int main(int argc, char **argv)
{
    unsigned short int a;
    signed short int b;
 
    a = 50000;
    b = 50000;
 
    printf("\n a = %d \t b = %d \n", a, b);
 
    return 0;


```



## LeetCode

[LeetCode-7. 整数反转-中等](https://leetcode.cn/problems/reverse-integer/) 

**Assume the environment does not allow you to store 64-bit integers (signed or unsigned).**



[LeetCode-260. 只出现一次的数字 III-中等](https://leetcode.cn/problems/single-number-iii/) 

[LeetCode-322. 零钱兑换-中等](https://leetcode.cn/problems/coin-change/) 

[LeetCode-437. Path Sum III-middle](https://leetcode.cn/problems/path-sum-iii/) 

[LeetCode-787. K 站中转内最便宜的航班](https://leetcode.cn/problems/cheapest-flights-within-k-stops/) 



### TO READ



Google： wraparound behavior overflow

https://github.com/google/or-tools/issues/410

https://www.gnu.org/software/gnulib/manual/html_node/Wraparound-Arithmetic.html

https://cwe.mitre.org/data/definitions/190.html



