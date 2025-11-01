# Machine epsilon

1、有了前面章节中关于"Accuracy problems"的讨论，现在理解本章的内容是容易的

2、epsilon可以看作是最小误差、相对误差，在各种物理中，也有类似的概念；它也可以视为computer的精度、准确的

## cnblogs [从如何判断浮点数是否等于0说起——浮点数的机器级表示](https://www.cnblogs.com/kubixuesheng/p/4107309.html)

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

## wikipedia [Machine epsilon](https://en.wikipedia.org/wiki/Machine_epsilon)

**Machine epsilon** gives an upper bound on the [relative error](https://en.wikipedia.org/wiki/Approximation_error) due to [rounding](https://en.wikipedia.org/wiki/Rounding) in [floating point arithmetic](https://en.wikipedia.org/wiki/Floating_point_arithmetic). This value characterizes [computer arithmetic](https://en.wikipedia.org/wiki/Computer_arithmetic) in the field of [numerical analysis](https://en.wikipedia.org/wiki/Numerical_analysis), and by extension in the subject of [computational science](https://en.wikipedia.org/wiki/Computational_science). The quantity is also called **macheps** or **unit roundoff**, and it has the symbols Greek [epsilon](https://en.wikipedia.org/wiki/Epsilon) $\epsilon$ or bold Roman **u**, respectively.

> NOTE: 
> 
> 1、 [relative error](https://en.wikipedia.org/wiki/Approximation_error) 即"相对误差"

### How to determine machine epsilon

Where standard libraries do not provide precomputed values (as `<` [float.h](https://en.wikipedia.org/wiki/Float.h) `>` does with `FLT_EPSILON`, `DBL_EPSILON` and `LDBL_EPSILON` for C and `<` [limits](https://en.wikipedia.org/wiki/C%2B%2B_Standard_Library#Language_support) `>` does with `std::numeric_limits<T>::epsilon()` in C++), the best way to determine machine epsilon is to refer to the table, above, and use the appropriate power formula. Computing machine epsilon is often given as a textbook exercise. The following examples compute machine epsilon in the sense of the spacing of the floating point numbers at 1 rather than in the sense of the unit roundoff.