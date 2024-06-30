# Float pointer number comparison

一、float pointer number有 +0 和 -0，因此，它和0的比较比较有技巧

二、查阅了一下，关于float pointer number的comparison，有多种写法，在下面文章中，对此进行了非常好的总结:

https://www.cygnus-software.com/papers/comparingfloats/comparingfloats.htm

https://randomascii.wordpress.com/category/floating-point/

https://randomascii.wordpress.com/2012/02/25/comparing-floating-point-numbers-2012-edition/

三、C++、C的[Numerics library](https://en.cppreference.com/w/cpp/numeric)也将对此进行了总结了: 

1、[std::isgreater](https://en.cppreference.com/w/cpp/numeric/math/isgreater)

2、[std::isless](https://en.cppreference.com/w/cpp/numeric/math/isless)



## bitbashing [Comparing Floating-Point Numbers Is Tricky](https://bitbashing.io/comparing-floats.html)

## stackoverflow [Compare double to zero using epsilon](https://stackoverflow.com/questions/13698927/compare-double-to-zero-using-epsilon)

Today, I was looking through some C++ code (written by somebody else) and found this section:

```cpp
#include <iostream>
#include <limits>

bool IsZero(double &v)
{
	if (v < std::numeric_limits<double>::epsilon() && v > -std::numeric_limits<double>::epsilon())
	{
		return true;
	}
	return false;
}
int main()
{
	double someValue = 0.0;
	std::cout << IsZero(someValue) << std::endl;
}
// g++ test.cpp

```

I'm trying to figure out whether this even makes sense.

The documentation for `epsilon()` says:

> The function returns the difference between 1 and the smallest value greater than 1 that is representable [by a double].

Does this apply to 0 as well, i.e. `epsilon()` is the smallest value greater than 0? Or are there numbers between `0` and `0 + epsilon` that can be represented by a `double`?

If not, then isn't the comparison equivalent to `someValue == 0.0`?





## cnblogs [从如何判断浮点数是否等于0说起——浮点数的机器级表示](https://www.cnblogs.com/kubixuesheng/p/4107309.html)

> NOTE: 
>
> 1、这篇文章重要是对float point number的format、epsilon进行分析，由于在其他章节中也将包含了这些内容，所以不再赘述

### Equal to zero/`isZero`

```C++
#include <iostream>
#include <limits>
#include <float.h>

bool isZero(double d)
{
	if (d >= -DBL_EPSILON && d <= DBL_EPSILON)
	{
		return true;
	}
	return false;
}

bool isZero(int d)
{
	if (0 == d)
	{
		return true;
	}
	return false;
}

bool isZero(int *d)
{
	if (NULL == d)
	{
		return true;
	}
	return false;
}

bool isZero(bool d)
{
	if (!d)
	{
		return true;
	}
	return false;
}
int main()
{
	double someValue = 0.0;
	std::cout << isZero(someValue) << std::endl;
}
// g++ test.cpp

```

### Equality



```C++
double dd = sin(3.141592653589793 / 6);
    /*if (dd == 0.5)
    {取决于不同的编译器或者机器平台……这样写，即使有时候是对的，但是就怕习惯，很容易出错。
    }*/

    if (fabs(dd - 0.5) < DBL_EPSILON)
    {
        //满足这个条件，我们就认为dd和0.5相等，否则不等
        puts("ok");//打印了ok
    }
```

## stackoverflow [Should we compare floating point numbers for equality against a *relative* error?](https://stackoverflow.com/questions/328475/should-we-compare-floating-point-numbers-for-equality-against-a-relative-error)











## Google: how to judge whether a double is bigger than zero C++

https://stackoverflow.com/questions/39040900/simple-way-to-check-if-double-value-is-greater-than-zero





## TODO

### GitHub: double



https://github.com/search?l=C%2B%2B&q=double&type=Repositories



https://github.com/google/double-conversion/blob/master/double-conversion/utils.h

### Google: check if a double is zero

https://stackoverflow.com/questions/18260213/how-to-test-if-a-double-is-zero/18260267

https://stackoverflow.com/questions/21416022/is-it-correct-to-compare-a-double-to-zero-if-you-previously-initialized-it-to-ze



### Google c++ double greater than 0

https://stackoverflow.com/questions/19837576/comparing-floating-point-number-to-zero



https://isocpp.org/wiki/faq/newbie#floating-pt-errs



https://randomascii.wordpress.com/2012/02/25/comparing-floating-point-numbers-2012-edition/