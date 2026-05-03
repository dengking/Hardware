# ULP&Machine epsilon&tolerance epsilon

## Unit in the last place(ULP)(末位单位)

`Unit in the last place`，简称 `ULP`，可以直译为：**最后一位上的单位**，或者更自然地说：**末位单位**。它是 IEEE 754 浮点数体系的核心概念，它表示：**某个数附近，相邻两个可表示浮点数之间的间隔。**

你可以把它理解成：**浮点数系统在这个量级上的“最小刻度”**。

#### 为什么叫 “last place”

这里的 `last place` 不是十进制字符串里最后一个字符，而是指：浮点数**有效数字**里 **最低有效位（least significant digit / bit）** 所代表的那个单位大小。IEEE 754 浮点数的标准结构为：`符号位 + 指数位 + 尾数位`，以最常用的 **双精度浮点数（double, 64位）** 为例：

- 1 位符号位，11 位指数位，52 位尾数位（含 1 位隐含的前导 1，总计 53 位**有效精度**）

- 一个正规浮点数的数值为：
  
  $$
  (-1)^s \times (1 + \text{尾数}) \times 2^{\text{指数}-1023}
  $$

在IEEE 754二进制浮点数里，“最后一位”的单位就是该数当前指数下**尾数最低位**所对应的数值。所以：

- 在小数尺度上(指数较小)，最末位可能非常小
- 在大数尺度上(指数较大)，最末位会更大

这也是为什么 `ULP` 会随着数值大小变化。

TODO: 这里需要补充IEEE 754双精度浮点数的ULP计算公式。

#### 直观理解: 十进制类比

假设某个系统只能表示：

```text
1.23, 1.24, 1.25, ...
```

那么在 `1.23` 附近，相邻数的间隔是：

$$
0.01
$$

所以这里的 `ulp(1.23)` 可以理解成 `0.01`。

如果换到只能表示：

```text
1230, 1240, 1250, ...
```

那么这时相邻数的间隔变成：

$$
10
$$

所以 `ULP` 是**随量级变化**的。

---

### 在浮点数里，ULP 到底是什么

对一个浮点数 $x$，`ulp(x)` 通常可以理解为：

$$
\mathrm{ulp}(x) \approx \text{the distance between adjacent floating-point numbers near } x
$$

也就是说：

- `ulp(1)`：看 `1` 附近两个相邻可表示浮点数差多少
- `ulp(1000)`：看 `1000` 附近相邻浮点数差多少
- `ulp(10^{-300})`：看极小数附近相邻浮点数差多少

因此 `ULP` 不是一个固定常数，而是一个**依赖位置的局部量**。

### 一个最常见的例子：`double` 精度

对于 IEEE 754 的 binary64，也就是常见的 `double`：

在 `1` 附近，相邻两个可表示数之间的距离是：

$$
\mathrm{ulp}(1) = 2^{-52} \approx 2.22 \times 10^{-16}
$$

这就是为什么很多地方会写：

- `machine epsilon ≈ 2.22e-16`

在这种定义下，

$$
\epsilon_{mach} = \mathrm{ulp}(1)
$$

但注意：

$$
\mathrm{ulp}(x) \neq \mathrm{ulp}(1)
$$

对一般的 $x$，它会变。

### 为什么数越大，ULP 往往越大

浮点数本质上类似科学计数法：

$$
x = m \times 2^e
$$

其中：

- $m$ 是尾数（significand）
- $e$ 是指数

尾数能存的位数固定，所以：

- 当指数 $e$ 变大时，
- 最低有效位对应的实际数值也会放大

因此：

- 在 `1` 附近，间隔很小
- 在 `2` 附近，间隔大一倍
- 在 `1024` 附近，间隔会更大

对 binary64 来说，大致规律是：

- 在区间 $[1, 2)$ 内，间隔约为 $2^{-52}$
- 在区间 $[2, 4)$ 内，间隔约为 $2^{-51}$
- 在区间 $[4, 8)$ 内，间隔约为 $2^{-50}$

每跨过一个 2 的幂，间隔翻倍。

### “误差是 1 ULP” 是什么意思

如果有人说：

> 某个计算结果误差是 `1 ulp`

意思通常是：

> 结果与正确舍入值相比，偏差大约等于一个相邻浮点数的间距。

也就是“差了一个可表示数的台阶”。

这是一种非常自然的误差度量，因为它不是按**绝对值**看，而是按**当前量级下的浮点分辨率**来看。

### ULP VS machine epsilon VS tolerance epsilon

这几个概念很容易混：

### `ULP(x)`

- 某个数 $x$ 附近的浮点间隔
- 是浮点表示系统的局部性质

### `machine epsilon`

- 一般指 `1` 附近的那个特殊间隔，或与之相关的量
- 是硬件/标准决定的

### `tolerance epsilon`

- 你在算法里自己设的停止阈值
- 例如二分法里 `if abs(f(x)) < epsilon`
- 和 `ULP` 不是一回事

### 三种误差

在数值误差分析里：

- **绝对误差**：看差值本身
- **相对误差**：看差值相对于真值的比例
- **ULP 误差**：看差值相对于“当前位置浮点刻度”的大小

这三者关注点不同：

- 绝对误差：物理量上差多少
- 相对误差：比例上差多少
- ULP：浮点表示层面差了几个台阶

### Application&example

1、在阅读 cppreference [`std::numeric_limits<T>::epsilon`](https://en.cppreference.com/w/cpp/types/numeric_limits/epsilon) 的example时，其中的例子有如下的code:

```cpp
template<class T>
typename std::enable_if<!std::numeric_limits<T>::is_integer, bool>::type almost_equal(T x, T y, int ulp)
{
    // the machine epsilon has to be scaled to the magnitude of the values used
    // and multiplied by the desired precision in ULPs (units in the last place)
    return std::fabs(x - y) <= std::numeric_limits<T>::epsilon() * std::fabs(x + y) * ulp
    // unless the result is subnormal
                    || std::fabs(x - y) < std::numeric_limits<T>::min();
}
```

#### 误差分析

数值误差分析、浮点鲁棒性设计的核心指标。

##### 衡量舍入误差

浮点运算结果一般不能精确表示，所以要舍入。 `ULP` 提供了一个“这个舍入误差到底大不大”的本地尺度。

在“round to nearest”下，通常可以认为：

> 正确舍入到最近浮点数时，误差不超过 `0.5 ulp`

这是很重要的结论。

一个简洁例子

假设某个简化浮点系统在 `1` 附近只能表示：

```text
1.000
1.001
1.002
1.003
```

那么：

$$
\mathrm{ulp}(1.000) = 0.001
$$

如果真实结果是 `1.0004`，舍入后变成 `1.000`，误差是：

$$
0.0004 = 0.4 \text{ ulp}
$$

如果真实结果是 `1.0006`，舍入后变成 `1.001`，误差也是：

$$
0.0004 = 0.4 \text{ ulp}
$$

所以 `ULP` 能很好表达“误差相对于本地分辨率有多大”。

#### 比较两个浮点结果是否“足够接近”

有时候直接比较绝对误差不合理，因为：

- 对很小的数，`1e-15` 很大
- 对很大的数，`1e-15` 又太小

这时用 `ULP` 比较会更自然：

> 两个结果是否只差几个可表示浮点数？

#### 理解机器精度

`machine epsilon` 和 `ULP` 关系很近。  
在常见定义下：

$$
\epsilon_{mach} = \mathrm{ulp}(1)
$$

所以理解 `ULP` 后，`machine epsilon` 也更容易理解。





### wikipedia [Unit in the last place](https://en.wikipedia.org/wiki/Unit_in_the_last_place)

In [computer science](https://en.wikipedia.org/wiki/Computer_science "Computer science") and [numerical analysis](https://en.wikipedia.org/wiki/Numerical_analysis "Numerical analysis"), **unit in the last place** or **unit of least precision** (**ulp**) is the spacing between two consecutive [floating-point](https://en.wikipedia.org/wiki/Floating-point "Floating-point") numbers, i.e., the value the *[least significant digit](https://en.wikipedia.org/wiki/Least_significant_digit "Least significant digit")* (rightmost digit) represents if it is 1. It is used as a measure of [accuracy](https://en.wikipedia.org/wiki/Accuracy_and_precision "Accuracy and precision") in numeric calculations.[[1]](https://en.wikipedia.org/wiki/Unit_in_the_last_place#cite_note-1)

## Machine epsilon

在前面已经提到了machine epsilon

### cnblogs [从如何判断浮点数是否等于0说起——浮点数的机器级表示](https://www.cnblogs.com/kubixuesheng/p/4107309.html)

EPSILON被规定为是**最小误差**，换句话说就是使得EPSILON+1.0不等于1.0的最小的正数，也就是如果正数d小于EPISILON，那么d和1.0相加，计算机就认为还是等于1.0，这个EPISILON是变和不变的临界值。

官方解释：

> For EPSILON, you can use the constants `FLT_EPSILON`, which is defined for float as 1.192092896e-07F, or DBL_EPSILON, which is defined for double as 2.2204460492503131e-016. You need to include `float.h` for these constants. These constants are defined as the smallest positive number x, such that x+1.0 is not equal to 1.0. Because this is a very small number, you should employ user-defined tolerance for calculations involving very large numbers.

**还有一个问题，逼逼了那么多，浮点数无法精确表达实数，那为啥epsilon的大小是尼玛那样的？**

```C++
1 #define DBL_EPSILON      2.2204460492503131E-16 
2 #define FLT_EPSILON     1.19209290E-07F 
3 #define LDBL_EPSILON     1.084202172485504E-19 
```

前面已经说了，数学上学的实数可以用数轴无穷尽的表示，但是计算机不行，在计算机中实数和浮点数还是不一样的，我个人理解。浮点数是属于有理数中某特定子集的数的数字表示，在计算机中用以近似表示任意某个实数。

在计算机中，整数和纯小数使用定点数表示，叫定点小数和定点正数，对混合有正数和小数的数，使用浮点数表示，所谓浮点，浮点数依靠小数点的浮动（因为有指数的存在）来动态表示实数。灵活扩大实数表达范围。但在计算过程中，难免丢失精度。

至于epsilon的大小，前面也贴出了官方定义，它就规定了，当x（假如x是双精度）落在了`+- DBL_EPSILON`之内，`x + 1.0 = 1.0`，就是这么规定的。x在此范围之内的话，都呗计算机认为是0.0 。

### wikipedia [Machine epsilon](https://en.wikipedia.org/wiki/Machine_epsilon)

**Machine epsilon** gives an upper bound on the [relative error](https://en.wikipedia.org/wiki/Approximation_error) due to [rounding](https://en.wikipedia.org/wiki/Rounding) in [floating point arithmetic](https://en.wikipedia.org/wiki/Floating_point_arithmetic). This value characterizes [computer arithmetic](https://en.wikipedia.org/wiki/Computer_arithmetic) in the field of [numerical analysis](https://en.wikipedia.org/wiki/Numerical_analysis), and by extension in the subject of [computational science](https://en.wikipedia.org/wiki/Computational_science). The quantity is also called **macheps** or **unit roundoff**, and it has the symbols Greek [epsilon](https://en.wikipedia.org/wiki/Epsilon) $\epsilon$ or bold Roman **u**, respectively.

> NOTE:  [relative error](https://en.wikipedia.org/wiki/Approximation_error) 即"相对误差"

#### How to determine machine epsilon

Where standard libraries do not provide precomputed values (as `<` [float.h](https://en.wikipedia.org/wiki/Float.h) `>` does with `FLT_EPSILON`, `DBL_EPSILON` and `LDBL_EPSILON` for C and `<` [limits](https://en.wikipedia.org/wiki/C%2B%2B_Standard_Library#Language_support) `>` does with `std::numeric_limits<T>::epsilon()` in C++), the best way to determine machine epsilon is to refer to the table, above, and use the appropriate power formula. Computing machine epsilon is often given as a textbook exercise. The following examples compute machine epsilon in the sense of the spacing of the floating point numbers at 1 rather than in the sense of the unit roundoff.

#### Role 1: Machine Epsilon ($\epsilon_{mach}$) - The Limit of the Hardware

This is a formal, defined quantity that describes the best possible precision of your computer's floating-point arithmetic.

* **Definition:** Machine Epsilon is the smallest positive number such that $1.0 + \epsilon_{mach} \neq 1.0$ on the computer. It's the gap between 1.0 and the very next number that can be represented.
* **What it tells you:** It quantifies the maximum **relative error** that can occur from a single rounding operation.
* **Typical Values:**
  * For 64-bit `double` precision: $\epsilon_{mach} \approx 2.22 \times 10^{-16}$
  * For 32-bit `float` precision: $\epsilon_{mach} \approx 1.19 \times 10^{-7}$

#### Machine epsilon vs. ULP

`Epsilon` and `ULP` are related, but they are **not the same concept**.

- **ULP** means **unit in the last place**. Informally, $\mathrm{ulp}(x)$ is the distance between two adjacent floating-point numbers near $x$.
- **Machine epsilon** is a specific quantity tied to the floating-point system.
- **Tolerance epsilon** is a user-chosen stopping threshold in a numerical algorithm, and has nothing to do with the spacing of representable floating-point numbers.

Under the definition used in this section,

$$
\epsilon_{mach} = \min \{ \epsilon > 0 : 1 + \epsilon \neq 1 \},
$$

so for IEEE 754 binary floating-point arithmetic,

$$
\epsilon_{mach} = \mathrm{ulp}(1).
$$

For example, in binary64 (`double`),

$$
\mathrm{ulp}(1) = 2^{-52} \approx 2.220446049250313 \times 10^{-16}.
$$

However, $\mathrm{ulp}(x)$ depends on the magnitude of $x$, so it is **not** a single global constant. Near larger numbers, the spacing is larger; near smaller numbers, the spacing is smaller. Roughly speaking, for normalized binary floating-point numbers in the interval $[2^e, 2^{e+1})$, the spacing is constant within that interval and doubles when crossing a power of two.

So the precise relationship is:

- `tolerance epsilon` $\neq$ `ULP`
- `machine epsilon = ulp(1)` under the definition used here
- `ULP(x)` is a local spacing near $x$, not a universal constant

There is also another closely related quantity called **unit roundoff**, often denoted by $u$. Under round-to-nearest,

$$
u = \frac{1}{2}\mathrm{ulp}(1).
$$

For binary64,

$$
u = 2^{-53} \approx 1.1102230246251565 \times 10^{-16}.
$$

Some textbooks use the phrase *machine epsilon* to mean $u$ instead of $\mathrm{ulp}(1)$, so when reading numerical analysis literature, it is important to check the exact definition being used.

A good short summary is:

> `ULP` is the spacing between adjacent floating-point numbers near $x$; `machine epsilon` is the special case at $x = 1$ under one common definition; `tolerance epsilon` is a user-defined error threshold.

## Epsilon in numerical error analysis

This is a fantastic question that gets to the very heart of **numerical analysis**. Explaining epsilon is crucial to understanding why numerical algorithms work the way they do.

翻译: 当然！这是一个直击数值分析核心的绝佳问题。理解机器精度 ε（epsilon），对于搞懂数值算法为何如此运行至关重要。

In numerical analysis, **epsilon ($\epsilon$)** is a small, positive number that represents a **tolerance for error**. It is the formal acknowledgment that computations on a computer are not exact. We use epsilon to define what it means for a result to be "good enough."

> 翻译: 在数值分析中，**ε（伊普西隆，符号记为\(\epsilon\)）** 是一个极小的正数，代表**误差容限**。它是对 “计算机计算并非绝对精确” 这一事实的正式认可。我们通过 ε 来定义 “结果达到何种程度可视为‘足够好~~~~’”。

Let's break this down from the ground up.

### 1. The Fundamental Problem: Computers Can't Do Perfect Math

The field of numerical analysis exists because computers have two fundamental limitations when representing real numbers:

1. **Finite Precision (Representation Error):** Computers use a finite number of bits (usually 64 bits for a `double`) to store numbers. This means they cannot represent all real numbers. Simple fractions like $1/3$ or even $1/10$ become repeating sequences in binary and must be truncated, leading to a small, initial error.
   
   * `0.1` in binary is `0.0001100110011...` (the `0011` repeats forever).
   * 参见 wikipedia [Round-off error](https://en.wikipedia.org/wiki/Round-off_error#)，其中说明了Representation error

2. **Rounding Error:** When you perform arithmetic (e.g., multiplication, addition), the result might require more precision than the computer can store. The result must be rounded, introducing another small error.
   
   1. 参见 wikipedia [Round-off error](https://en.wikipedia.org/wiki/Round-off_error#)，

This leads to the classic problem:

```python
>>> 0.1 + 0.2 == 0.3
False  # The result is actually 0.30000000000000004
```

Because of this, we can almost never ask "Is $x$ equal to $y$?". Instead, we must ask, **"Is $x$ close enough to $y$?"** Epsilon defines "close enough."

---

### 2. The Two Main Roles of Epsilon in Error Analysis

Epsilon is not a single concept; it plays two distinct but related roles.

#### Role 1: Machine Epsilon ($\epsilon_{mach}$) - The Limit of the Hardware

This is a formal, defined quantity that describes the best possible precision of your computer's floating-point arithmetic.

* **Definition:** Machine Epsilon is the smallest positive number such that $1.0 + \epsilon_{mach} \neq 1.0$ on the computer. It's the gap between 1.0 and the very next number that can be represented.
* **What it tells you:** It quantifies the maximum **relative error** that can occur from a single rounding operation.
* **Typical Values:**
  * For 64-bit `double` precision: $\epsilon_{mach} \approx 2.22 \times 10^{-16}$
  * For 32-bit `float` precision: $\epsilon_{mach} \approx 1.19 \times 10^{-7}$

In error analysis, $\epsilon_{mach}$ is the fundamental unit of round-off error. When analyzing an algorithm, you can say that after $N$ operations, the total accumulated **round-off error** will be roughly on the order of $N \times \epsilon_{mach}$ (this is a simplification, but it captures the idea).

#### Role 2: Tolerance Epsilon ($\epsilon_{tol}$) - The Goal of the Algorithm

This is a user-defined value that you, the programmer or analyst, choose. It sets the target accuracy for your algorithm.

* **Definition:** A user-specified threshold to decide when an algorithm has produced an acceptable result.
* **Use Case:** It's used primarily in **stopping conditions** for iterative algorithms.

---

### 3. Epsilon in Action: Controlling Numerical Algorithms

Numerical analysis is often about trading accuracy for computation time. Epsilon is the knob you turn to control this trade-off.

Let's look at two classic examples:

#### Example A: Finding a Root with the Bisection Method

**Goal:** Find an $x$ such that $f(x) = 0$.
The bisection method repeatedly narrows an interval $[a, b]$ that is known to contain a root. We can't wait until we find an $x$ where $f(x)$ is *exactly* zero, because that may never happen due to floating-point errors.

Instead, we use epsilon for our stopping conditions. We stop when:

1. **The interval is small enough:** The length of the interval $(b - a)$ is less than some tolerance. This guarantees the error in our root's location is small.
   * `if (b - a) < epsilon_x:` stop.
2. **The function value is close enough to zero:**
   * `if abs(f(midpoint)) < epsilon_f:` stop.

Here, `epsilon_x` and `epsilon_f` are user-defined tolerances. A smaller epsilon gives a more accurate answer but requires more iterations.

#### Example B: Numerical Integration (e.g., Adaptive Quadrature)

**Goal:** Calculate the definite integral $I = \int_a^b f(x) dx$.
Adaptive methods calculate the integral on a coarse grid, then estimate the error. If the error is larger than a tolerance $\epsilon$, the algorithm refines the grid (divides it into smaller pieces) only where the function is changing rapidly and recalculates.

* The algorithm stops when the estimated error in the integral calculation is less than the user's desired tolerance, $\epsilon$.
* Epsilon directly controls the balance: a small $\epsilon$ forces the algorithm to use many small steps, yielding high accuracy at a high computational cost.

### 4. Absolute vs. Relative Tolerance: A Crucial Distinction

When choosing a tolerance epsilon, you must consider the scale(量级) of your numbers.

* **Absolute Tolerance:** `if abs(a - b) < epsilon_abs:`
  
  * **Good for:** When you know your numbers will be close to zero, or when you have a specific physical meaning for the tolerance (e.g., "accurate to 1 millimeter," so `epsilon_abs = 0.001`).
  * **Fails when:** Your numbers are very large. For `a = 1,000,000` and `b = 1,000,001`, `abs(a-b)` is 1.0, which is likely much larger than a typical `epsilon_abs`, even though the numbers are very close in a relative sense.

* **Relative Tolerance:** `if abs(a - b) < epsilon_rel * abs(b):` (or some variation)
  
  * **Good for:** General-purpose comparisons where the magnitude of numbers can vary widely. It checks if `a` and `b` are close *relative to their size*.
  * **Fails when:** Your numbers are very close to zero. If `b` is near zero, the threshold `epsilon_rel * abs(b)` becomes tiny, possibly smaller than machine epsilon, making the comparison fail.

**The Best Practice:** Most robust numerical libraries (like NumPy) use a combination of both:
`if abs(a - b) <= epsilon_abs + epsilon_rel * abs(b):`
This ensures the check works well for numbers near zero (where the absolute tolerance dominates) and for large numbers (where the relative tolerance dominates).

### Summary

| Concept                                  | Role in Error Analysis                                                                           | Who Defines It?                                         |
|:---------------------------------------- |:------------------------------------------------------------------------------------------------ |:------------------------------------------------------- |
| **Machine Epsilon ($\epsilon_{mach}$)**  | The fundamental unit of **round-off error**. It's the best possible precision of the hardware.   | The computer hardware/architecture (IEEE 754 standard). |
| **Tolerance Epsilon ($\epsilon_{tol}$)** | A target for **truncation error** or a **stopping criterion**. It defines what is "good enough." | The user/programmer of the numerical algorithm.         |

In essence, **error analysis** in numerical methods is the study of two primary errors:

1. **Round-off Error:** Unavoidable noise from the hardware, quantified by $\epsilon_{mach}$.
2. **Truncation Error:** Error from the algorithm itself (e.g., using a finite number of steps). We use $\epsilon_{tol}$ to control this error and decide when it's small enough to stop.
