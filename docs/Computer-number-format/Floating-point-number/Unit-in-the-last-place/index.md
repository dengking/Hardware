# Unit in the last place

1、在阅读 cppreference [`std::numeric_limits<T>::epsilon`](https://en.cppreference.com/w/cpp/types/numeric_limits/epsilon) 的example时，其中的例子有如下的code:

````C++
template<class T>
typename std::enable_if<!std::numeric_limits<T>::is_integer, bool>::type almost_equal(T x, T y, int ulp)
{
	// the machine epsilon has to be scaled to the magnitude of the values used
	// and multiplied by the desired precision in ULPs (units in the last place)
	return std::fabs(x - y) <= std::numeric_limits<T>::epsilon() * std::fabs(x + y) * ulp
	// unless the result is subnormal
					|| std::fabs(x - y) < std::numeric_limits<T>::min();
}
````

其中，提及了"ULPs (units in the last place)"，那这是什么呢？

## wikipedia [Unit in the last place](https://en.wikipedia.org/wiki/Unit_in_the_last_place)