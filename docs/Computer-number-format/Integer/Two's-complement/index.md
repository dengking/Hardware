# Two's complement

一、一些素材:

1、在`Book-计算机组成原理-科学出版社-2-运算方法与运算器`中，对complement进行了非常好的说明，可以参看其中的内容，因此，本文不对原理进行展开。

2、`x&(-x)`

二、"取反+1"中的"+1"相当于是火药，能够保证两数之和为0

三、

```C++
INT_MAX   = 0x7FFF=01111111111111111111111111111111(最大)
        1 = 0x0001=00000000000000000000000000000001
        0 = 0x0000=00000000000000000000000000000000
       -1 = 0xFFFF=11111111111111111111111111111111(最大)
INT_MIN+1 = 0x8001=10000000000000000000000000000001         
INT_MIN   = 0x1000=10000000000000000000000000000000         
```



## wikipedia [Two's complement](https://en.wikipedia.org/wiki/Two%27s_complement)

**Two's complement** is a [mathematical operation](https://en.wikipedia.org/wiki/Mathematical_operation) on [binary numbers](https://en.wikipedia.org/wiki/Binary_number), and is an example of a [radix complement](https://en.wikipedia.org/wiki/Method_of_complements)(基数). It is used in [computing](https://en.wikipedia.org/wiki/Computing) as a method of [signed number representation](https://en.wikipedia.org/wiki/Signed_number_representation).

The two's complement of an *N*-bit number is defined as its [complement](https://en.wikipedia.org/wiki/Method_of_complements) with respect to $2^N$. For instance, for the three-bit number $010$, the two's complement is $110$, because $010 + 110 = 1000$.

> NOTE: $1000$所表示的是$2^N$。求解的方法也非常简单：取反加1；



Two's complement is the most common method of representing signed [integers](https://en.wikipedia.org/wiki/Integer_(computer_science)) on computers,[[1\]](https://en.wikipedia.org/wiki/Two's_complement#cite_note-1) and more generally, [fixed point binary](https://en.wikipedia.org/wiki/Fixed-point_arithmetic) values. In this scheme, if the binary number $010_2$ encodes the signed integer $2_{10}$, then its two's complement, $110_2$, encodes the inverse: $−2_{10}$. In other words, to reverse the sign of any integer in this scheme, you can take the two's complement of its binary representation.[[2\]](https://en.wikipedia.org/wiki/Two's_complement#cite_note-2) The tables at right illustrate this property.

Compared to other systems for representing signed numbers (*e.g.,* [ones' complement](https://en.wikipedia.org/wiki/Ones'_complement)), two's complement has the advantage that the fundamental arithmetic operations of [addition](https://en.wikipedia.org/wiki/Addition), [subtraction](https://en.wikipedia.org/wiki/Subtraction), and [multiplication](https://en.wikipedia.org/wiki/Multiplication) are identical to those for unsigned binary numbers (as long as the inputs are represented in the same number of bits - as the output, and any [overflow](https://en.wikipedia.org/wiki/Integer_overflow) beyond those bits is discarded from the result). This property makes the system simpler to implement, especially for higher-precision arithmetic. Unlike ones' complement systems, two's complement has no representation for [negative zero](https://en.wikipedia.org/wiki/Ones'_complement#Negative_zero), and thus does not suffer from its associated difficulties.

Conveniently, another way of finding the two's complement of a number is to take its ones' complement and add one: the sum of a number and its ones' complement is all '1' bits, or 2*N* − 1; and by definition, the sum of a number and its *two's* complement is 2*N*.

> NOTE: 
>
> 1、$N$位二进制串，能够编码的数的总个数是:$2^N$；比如对于`int`，它是32位，所以能够编码$2^{32}$个数，由于设计者的目的是它是有符号的，即它需要能够即表示正数，也要能够表示负数，即它需要有**符号位**；其次，由于[complement](https://en.wikipedia.org/wiki/Method_of_complements)的优势：

- encode a symmetric range of positive and negative [integers](https://en.wikipedia.org/wiki/Integer) in a way that they can use the same [algorithm](https://en.wikipedia.org/wiki/Algorithm) (hardware) for [addition](https://en.wikipedia.org/wiki/Addition)throughout the whole range.

所以，它最终采用[Two's complement](https://en.wikipedia.org/wiki/Two%27s_complement)来进行编码；对于`int`，它有32位， two's complement of an *32*-bit number is defined as its [complement](https://en.wikipedia.org/wiki/Method_of_complements) with respect to $2^{32}$.那怎样来理解complement和求解complement呢？其实对于complement的理解要从这个运算的本质来进行理解，在[Method of complements](https://en.wikipedia.org/wiki/Method_of_complements)中对其进行了非常详细的分析；







## stackoverflow [Why prefer two's complement over sign-and-magnitude for signed numbers?](https://stackoverflow.com/questions/1125304/why-prefer-twos-complement-over-sign-and-magnitude-for-signed-numbers)

I'm just curious if there's a reason why in order to represent -1 in binary, two's complement is used: flipping the bits and adding 1?

`-1` is represented by `11111111` (two's complement) rather than (to me more intuitive) `10000001` which is binary 1 with first bit as negative flag.

> NOTE: 取反加一

Disclaimer: I don't rely on binary arithmetic for my job!

### [A](https://stackoverflow.com/a/1125317)

It's done so that addition doesn't need to have any special logic for dealing with negative numbers. Check out [the article on Wikipedia](http://en.wikipedia.org/wiki/Two's_complement).

Say you have two numbers, 2 and `-1`. In your "intuitive" way of representing numbers, they would be `0010` and `1001`, respectively (I'm sticking to 4 bits for size). In the two's complement way, they are `0010` and `1111`. Now, let's say I want to add them.

> NOTE: 上面这段话对两种方式进行了对比；

Two's complement addition is very simple. You add numbers normally and any **carry bit** at the end is discarded. So they're added as follows:

```
  0010
+ 1111
=10001
= 0001 (discard the carry)
```

`0001` is 1, which is the expected result of "2+(-1)".

But in your "intuitive" method, adding is more complicated:

```
  0010
+ 1001
= 1011
```

Which is `-3`, right? Simple addition doesn't work in this case. You need to note that one of the numbers is negative and use a different algorithm if that's the case.

For this "intuitive" storage method, subtraction is a different operation than addition, requiring additional checks on the numbers before they can be added. Since you want the most basic operations (addition, subtraction, etc) to be as fast as possible, you need to store numbers in a way that lets you use the simplest algorithms possible.

Additionally, in the "intuitive" storage method, there are two zeroes:

```
0000  "zero"
1000  "negative zero"
```

Which are intuitively the same number but have two different values when stored. Every application will need to take extra steps to make sure that non-zero values are also not negative zero.

There's another bonus with storing ints this way, and that's when you need to extend the width of the register the value is being stored in. With two's complement, storing a 4-bit number in an 8-bit register is a matter of repeating its most significant bit:

```
    0001 (one, in four bits)
00000001 (one, in eight bits)
    1110 (negative two, in four bits)
11111110 (negative two, in eight bits)
```

It's just a matter of looking at the sign bit of the smaller word and repeating it until it pads the width of the bigger word.

With your method you would need to clear the existing bit, which is an extra operation in addition to padding:

```
    0001 (one, in four bits)
00000001 (one, in eight bits)
    1010 (negative two, in four bits)
10000010 (negative two, in eight bits)
```

You still need to set those extra 4 bits in both cases, but in the "intuitive" case you need to clear the 5th bit as well. It's one tiny extra step in one of the most fundamental and common operations present in every application.

## stackoverflow [Why Two's Complement?](https://stackoverflow.com/questions/6853524/why-twos-complement)



### [A](https://stackoverflow.com/a/6853536)

> 1. How do computers represent negative numbers?

Take the positive value, invert all bits and add one.

> 2. Why do computers represent negative numbers this way?

It makes easy to add 7 in -7 and came up with a zero. The bit operations are fast.

------

**How does it make it easy?**

Take the 7 and -7 example. If you represent 7 as `00000111`, to find -7 invert all bits and add one:

```
11111000 -> 11111001
```

Now you can add following standard math rules:

```
  00000111
+ 11111001
-----------
  00000000
```

For the computer this operation is relatively easy, as it involves basically comparing bit by bit and carrying one.

If instead you represented -7 as `10000111`, this won't make sense:

```
  00000111
+ 10000111
-----------
  10001110 (-14)
```

To add them, you will involve more complex rules like analyzing the first bit, and transforming the values.

And don't forget what @trashgod said, in 2's complement you have only one zero. To check it:

> `00000000`
> `11111111` (invert all bits)
> `00000000` (add one)

Differently from `00000000` (*0*) being equal to `10000000` (*-0*)





## wikipedia [Method of complements](https://en.wikipedia.org/wiki/Method_of_complements)

In [mathematics](https://en.wikipedia.org/wiki/Mathematics) and [computing](https://en.wikipedia.org/wiki/Computing), the **method of complements** is a technique to encode a symmetric(对称的) range of positive and negative [integers](https://en.wikipedia.org/wiki/Integer) in a way that they can use the same [algorithm](https://en.wikipedia.org/wiki/Algorithm) (hardware) for [addition](https://en.wikipedia.org/wiki/Addition) throughout the whole range(因为计算机只有加法器). For a given number of [places](https://en.wikipedia.org/wiki/Positional_notation)(数位) half of the possible representations of numbers encode the **positive numbers,** the other half represents their respective [additive inverses](https://en.wikipedia.org/wiki/Additive_inverse) (加法逆元，其实就是相反数). The pairs of mutually **additive inverse** numbers are called *complements*（补）.  Thus [subtraction](https://en.wikipedia.org/wiki/Subtraction) of any number is implemented by adding its **complement**. Changing the **sign**（符号） of any number is encoded by generating its **complement**, which can be done by a very simple and efficient algorithm(最常见的应用就是得到一个数的负数). This method was commonly used in [mechanical calculators](https://en.wikipedia.org/wiki/Mechanical_calculator) and is still used in modern [computers](https://en.wikipedia.org/wiki/Computers). The generalized concept of the *radix complement*（基数补数（ (as described below) is also valuable in [number theory](https://en.wikipedia.org/wiki/Number_theory), such as in [Midy's theorem](https://en.wikipedia.org/wiki/Midy%27s_theorem).



> NOTE: 如何理解complement运算？下面这一段话是摘自《计算机组成原理 第五版 科学出版社》的chapter 2.1.2 数的机器码表示：
>
> 我们先以钟表对时为例说明补码的概念。假设现在的标志时间是4点整，而有一只表已经7点了，为例校准时间，可以采用两种方法：
>
> - 将时针退$7-3=4$格；
> - 将时针向前拨$12-3=9$格
>
> 这两种方法都能够对准到4点，由此可以看出，减3和加9是等价的。也就是说9是(-3)对12的补码，可以使用数学公式表示为
>
> $ -3 = +9 (mod 12)$
>
> $mod12$的意思就是12为模数，这个“模”表示丢掉的数值。上式在数学上称为**同余式**。
>
> 上式中，其所以$7-3$和$7+9 (mod12)$等价，原因就是表的时钟超过12时，将12自动丢掉，最后得到$16-12=4$。
>
> 其实，结合上面的解释可以看出互补的两个数，**符号**相反，两者的**绝对值**之和等于*radix* ；上面时钟的例子中，*radix* 就是12；所以求一个数的补码是非常简单的，就是根据前面的关系求解出来；
>
> 对于时钟而言，它能够表示的范围是1-12，即*radix*是12；对于计算机中的`int`等而言，它的*radix*是$2^{32}$。
>
> 看了上面的描述，难道component就是additive inverse，即opposite？