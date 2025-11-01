# Floating-point arithmetic

wikipedia [Round-off error](https://en.wikipedia.org/wiki/Round-off_error)

## is zero(判断浮点数是否为0)

floating point number有 +0 和 -0之分，且存在精度问题，因此，它和0的比较比较有技巧。

二、查阅了一下，关于float pointer number的comparison，有多种写法，在下面文章中，对此进行了非常好的总结:

https://www.cygnus-software.com/papers/comparingfloats/comparingfloats.htm

https://randomascii.wordpress.com/category/floating-point/

https://randomascii.wordpress.com/2012/02/25/comparing-floating-point-numbers-2012-edition/

三、C++、C的[Numerics library](https://en.cppreference.com/w/cpp/numeric)也将对此进行了总结了:

1、[std::isgreater](https://en.cppreference.com/w/cpp/numeric/math/isgreater)

2、[std::isless](https://en.cppreference.com/w/cpp/numeric/math/isless)
