# Subnormal floating point number

1、在 cppreference [std::numeric_limits<T>::epsilon](https://en.cppreference.com/w/cpp/types/numeric_limits/epsilon) 的例子中:

```C++
include <cmath>
#include <limits>
#include <iomanip>
#include <iostream>
#include <type_traits>
#include <algorithm>
 
template<class T>
typename std::enable_if<!std::numeric_limits<T>::is_integer, bool>::type
    almost_equal(T x, T y, int ulp)
{
    // the machine epsilon has to be scaled to the magnitude of the values used
    // and multiplied by the desired precision in ULPs (units in the last place)
    return std::fabs(x-y) <= std::numeric_limits<T>::epsilon() * std::fabs(x+y) * ulp
        // unless the result is subnormal
        || std::fabs(x-y) < std::numeric_limits<T>::min();
}
 
int main()
{
    double d1 = 0.2;
    double d2 = 1 / std::sqrt(5) / std::sqrt(5);
    std::cout << std::fixed << std::setprecision(20) 
        << "d1=" << d1 << "\nd2=" << d2 << '\n';
 
    if(d1 == d2)
        std::cout << "d1 == d2\n";
    else
        std::cout << "d1 != d2\n";
 
    if(almost_equal(d1, d2, 2))
        std::cout << "d1 almost equals d2\n";
    else
        std::cout << "d1 does not almost equal d2\n";
}
```

上面提及了"subnormal"，这引起了我的注意:

`std::fabs(x-y) < std::numeric_limits<T>::min()`

显然，说明`std::fabs(x-y)` 超过了表示范围，在 stackoverflow [What is a subnormal floating point number?](https://stackoverflow.com/questions/8341395/what-is-a-subnormal-floating-point-number) # [A](https://stackoverflow.com/a/8341489) 中提出了这个观点。

 

## stackoverflow [What is a subnormal floating point number?](https://stackoverflow.com/questions/8341395/what-is-a-subnormal-floating-point-number)



[A](https://stackoverflow.com/a/8341489)

In the IEEE754 standard, floating point numbers are represented as binary scientific notation, *x* = *M* × 2*e*. Here *M* is the *mantissa* and *e* is the *exponent*. Mathematically, you can always choose the exponent so that 1 ≤ *M* < 2.* However, since in the computer representation the exponent can only have a finite range, there are some numbers which are bigger than zero, but smaller than 1.0 × 2*e*min. Those numbers are the *subnormals* or *denormals*.

Practically, the mantissa is stored without the leading 1, since there is always a leading 1, *except* for subnormal numbers (and zero). Thus the interpretation is that if the exponent is non-minimal, there is an implicit leading 1, and if the exponent is minimal, there isn't, and the number is subnormal.

*) More generally, 1 ≤ *M* < *B* for any base-*B* scientific notation.

> NOTE: 
>
> 1、简而言之，就是超过了可以表示的范围

[A](https://stackoverflow.com/a/53203428)

> NOTE: 
>
> 1、看了这个解释，非常复杂



## wikipedia [Denormal number](https://en.wikipedia.org/wiki/Denormal_number)

> NOTE: 
>
> 1、"denormal"的含义是: 非正规的



