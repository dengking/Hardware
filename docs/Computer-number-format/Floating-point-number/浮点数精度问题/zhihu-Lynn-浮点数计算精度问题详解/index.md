# zhihu [浮点数计算精度问题详解](https://zhuanlan.zhihu.com/p/467157712) 

你是否留意过 `0.1 + 0.2` 在计算机中等于多少？如果没有注意过的话，你可以立刻尝试打印一下「`0.1 + 0.2 == 0.3`」的结果。无论你使用的是 Js，Golang，或者是 Java，C，你都能得到一个反直觉的结果，**`0.1 + 0.2 == 0.3` 返回的是 false。**为什么计算机连如此基础的计算都会算错？在实际工作中，我们能信任计算机的计算结果吗？本文将从 0.1 + 0.2 展开，解释为什么浮点数的计算存在误差，并向读者展示计算误差在何种情况下会累积到不可接受的程度，并提供一种简单的**估计误差**的方法论，期望在设计系统时，这个方法论能帮我们提前考虑到计算误差的风险，并在系统设计时就能规避这样的风险。

```c++
#include <iostream>

int main() {

    std::cout << (0.1 + 0.2 == 0.3) << std::endl;
    double j = 0.1 + 0.2;
    std::cout << (j == 0.3) << std::endl;
    return 0;
}
// g++ test.cpp --std=c++11 -pedantic -Wall -Wextra

```

## 浮点数标准 IEEE 754

既然要说浮点数的问题，那就不得不提起 IEEE 754 标准，几乎所有的现代的 CPU 都采用这个标准。它决定了浮点数如何存储，如何计算。不过，在介绍 IEEE 754 之前，我们先从数学的角度简单看一下，计算机浮点数标准面临的问题。

### 从数学到计算机

**数学是一门抽象的学科，而计算机会面临着具体的实现**。我们的浮点数标准就是在做这样一件事：**建立计算机中存储的一个01序列到抽象数学中的一个数字的一对一的映射。**而这里有一个无法解决的问题，数学中任意小段连续的数轴中的小数就是**无限**的，这意味着我们需要**无限**个互不相同的01序列（一对一的映射）才能表示。但是，现实的计算机中，我们没有**无限**的内存，**有限**的内存就意味着我们只能表示有限的数字。这种**有限**与**无限**的割裂，导致任何浮点数标准都一定存在大量被舍弃的数字，大量在标准中不能精确表示的数字。

简单而言，从数学角度说，**无论任何浮点数标准，都面临用有限的集合去映射无限的集合的问题，都一定会存在精度问题。**而 IEEE 754 作为在六七十年代各种各样浮点数实现中胜出的标准，具有足够的合理性和先进性，并且在现在已经成为工业界的事实标准。

> NOTE:
>
> 一、float point number的精度问题是inherent的，是无法避免的

很多不了解计算精度问题的人，在遇到问题后，可能会抱怨标准，但事实上，浮点数的计算精度问题，既不是编程语言的问题，也不是标准的问题，而是**现实约束下的数学悖论**，无论任何**浮点数**的实现，都会面临类似的问题。只有我们理解这些问题，我们才能在写代码，系统设计时合理的去规避可能的精度问题。

### IEEE 754

在 IEEE 754 标准下，一个**浮点数**会以**科学计数法**的形式进行存储，如下图所示：

![img](https://pic2.zhimg.com/80/v2-eb251f39ad7099401982d2beda78aae5_1440w.webp)

其中，最高位一位是**符号位**，用于区分正负，中间 exponent 代表**指数部分**，低位 fraction 存储**有效数字**，可以形象地理解 fraction 决定了数字是哪个数字，exponent 决定了小数点在哪。fraction 这部分是后文核心，后文以「**有效位**」代指 fraction 所占用的存储空间，同时为了叙述方便，用「**浮点位**」代指 exponent 所占用的存储空间。

有效位的位数取决于具体使用的浮点数标准，单精度浮点数（float）中，我们有 23 位有效位，8位浮点位，而在双精度浮点数中，则有 52 位有效位，11 位浮点位。

由于浮点数精度问题核心是因为内存限制，也就是存储导致的，IEEE 754 其他方面的标准我们不再额外介绍，在了解了浮点数是如何存储的之后，我们便探索一下，计算误差是怎么出现的。

## 计算误差是如何产生的

我们都知道，计算机中，数字都是以二进制的形式存储的，在我们最初的例子中，0.1 和 0.2 虽然在十进制中是一个有限小数，但是转化为二进制后，却是一个无限小数，这导致了在存储 0.1 和 0.2 时，其实已经存在一定的精度丢失，而这样计算结果自然也存在误差。那我们能下结论说精度问题是因为参与运算的数值不能精确存储导致的吗？

### 计算误差的产生

事实上是不能的，我们可以看一个例子，在例子中，我们会使用十进制，因为这更符合我们的直觉，同时也不影响问题的阐述。假设我们在使用一个只有三位有效位的标准，现在，我们看一下计算「10 + 3.14 + 2.71」的结果，很明显，参与运算的所有数字都可以通过三位有效位来精确表达。我们精确计算的结果是 15.85，四舍五入保留三位有效位，结果是 15.9。现在，我们看一下计算机的计算结果：

![img](https://pic3.zhimg.com/80/v2-99b3a81538a5f0c96475fd3ac796f09a_1440w.webp)

可以看到，计算机的计算结果是 15.8，相比精确计算的结果，计算机出现了误差。这也告诉我们，**计算误差并不是简单因为参与计算的项不能精确表示，其本质原因，还是有效位位数的限制**。**并不是所有参与计算的数值都是精确的，结果就一定是精确的。**

到目前为止，我们看到的计算误差都很小，似乎就出现在最低位，那么，我们能放心地说，浮点数计算的误差是很小的，可以忽略的吗？

### 计算误差的积累

为了说明误差大小的问题，我们来看一个极端一点的例子，假设现在只有两位有效位，于是我们的浮点数标准能表示的最大数值是 99，最小精度是 0.01。现在我们计算一下「10+ 0.11 + 0.11 + ... + 0.11」，总共 100 个 0.11。我们精确计算的结果是 10 + 11 = 21。我们再看一下计算机的结果：

![img](https://pic1.zhimg.com/80/v2-c281a7cf4ada924f61a7d58e094ea90c_1440w.webp)

可以看到一个神奇的现象，在我们的浮点数标准下，10 + 0.11 永远等于 10， 10 相比于 21，误差是 11，而 11 已经是两位有效位下能表示的最高位数（十位）。进一步，我们继续增加 0.11 的数量，这个误差还会持续增大，甚至可以超过浮点数能够表示的最大数值 99。这种规模的误差，可就一点都不“小”。这也说明，浮点数的计算误差是不能随便忽略的。



## 如何估计系统中可能的计算误差

我们已经看到**浮点数**的**计算误差**可能很非常巨大，而在我们平时的工作中，仍然会有大量场景充斥着浮点数计算，它们都有问题吗？我们能信任浮点数的计算结果吗？

### 误差估计

这些问题我们可以拆开来看：

首先浮点数运算作为计算机世界的基础，如果它如此不堪，这个世界恐怕也很难支撑起一次工业革命级别的变革。
其次，从原理上我们也可以看到，浮点数计算的精度问题从数学层面就是不可避免的，现实中我们可以通过整数或者定点数来规避，但是这会带来其他额外的问题（溢出，性能还有程序复杂度），但单就浮点数计算的层面，这个误差是不可避免的。

归根结底，在误差一定存在的情况下，我们就需要去了解误差的影响，评估我们的系统是否存在误差风险？以及这种误差会影响业务吗？于是，对于涉及大量浮点数计算的系统，我们需要能够简单有效的评估，系统可能达到的误差是不是会超过业务可以接受的范围。我们先直接给出结论，然后再看一下具体的证明。

### 结论

为了估计系统面临的误差风险，我们需要估计如下三个量，然后结合**浮点数有效位**的限制，做出估计：

1. **a** - 系统中可能涉及的最大整数数值

2. **b** - 系统要求的精度（比如小数点后 6 位）

3. **c** - 系统的运算规模

   > NOTE:
   >
   > 一、$c$ 是什么含义？从后面的内容可知，它表示的是计算次数

4. **n** - 浮点数有效位的位数

> NOTE:
>
> 一、浮点数有效位就是前面说的fraction

然后通过下述公式计算，注意后文中所有的 log 都是以 2 为底：
$$
n+1 \leq log_2{a} + log_2{b} + log_2{c}
$$

> NOTE:
>
> 一、为什么log都是以2为底？因为计算机是以二进制来进行存储的，显然计算以2为底的对数是能够计算出位数的

如果上述不等式成立，则系统面临误差风险，右侧比左侧超出的越多，则出现不可接受范围的误差的可能性越大。

### 论证

首先，我们将浮点数的位做一下划分，如下图所示：

![img](https://pic1.zhimg.com/80/v2-b67bc88428e879b267e4ae6d4ca267c0_1440w.webp)

高位 X 位称作**精确位**，这部分就是业务要求，需要保证精确的位数，它实际上由**整数部分**和**小数部分**组成，也就是结论中的 $a$ 和 $b$，通俗的说，$a$ 确定了在最坏情况下，**整数部分**需要多少位才能表示，$b$ 则是由业务决定需要保证的**精度**。而 $X$ 就是精确表示的 $a.b$ 这个数所需要的位数。我们可以很容易有下推论：
$$
X = log_2{a} + log_2{b}
$$
低位 $Y$ 位被称作**误差位**，这部分就是我们可以允许存在误差的地方，只要误差不超过 $Y$ 位，我们的系统就不存在误差风险。接下来就需要我们对 $Y$ 做出评估。

可以回忆一下 「10 + 0.11 + ...」的例子，我们可以看到，随着计算次数的增加，**误差**在持续的扩大，这也告诉我们，计算次数是会影响到误差的。但是具体怎么影响，我们需要能够量化，否则我们也无法估计误差。我们先看一下，单次计算时的误差情况如下图：

![img](https://pic1.zhimg.com/80/v2-6db45e97dd16d325c4e75e7cd87f0024_1440w.webp)

从图中可以看到，在单次计算时，由于存储限制而被舍弃的精度 $z$，一定小于计算结果高位全是 0，只有最低位是 1 的数值，我们用大写 $Z$ 记录这个只有最低位是 1 的值，由于被舍弃的精度一定小于 $Z$，我们可以用 $Z$ 来约束单次计算的误差，即单次计算的误差 $z \textless Z$ 是恒成立的。

> NOTE:
>
> 一、上述 $Z$ 指的是??，下面是一个例子
>
> $Z=0.1$ 
>
> z=0.09999

而在最坏的情况下，计算结果的精确位部分就需要 $X$ 位，并且每一次计算所产生的的误差是累积的，不会互相抵消。这种情况下，误差的积累速度是最快的。我们用这种情况下只有最低位是 1 的 Z 来估计所有计算的误差。我们很容易能发现，对于任意一次计算的误差 z，$z \textless Z$ 是恒成立的。因为如果不成立，那么计算的结果一定出现了比 a 更大的数值，这不符合我们的假设。

现在，**单次计算**的误差不超过 $Z$，而我们允许的误差位只有 $Y$ 位，于是，我们可以很容易得出，当
$$
2^{Y+1} \le c \\ => Y+1 \le log_2{c}
$$
即，计算次数达到了 2 的 Y + 1 次方时，此时，在最坏的情况下，我们的误差需要 Y + 1 位才能表示，这种情况，系统就会面临误差风险。
