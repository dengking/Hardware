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

### Gemini 2.5 Pro: how to test if a floating point number is zero in python、java、c++

Of course. Testing if a floating-point number is zero is a classic problem in computer science because direct comparison (`==`) can be unreliable due to tiny precision errors from calculations.

The correct and robust method is to check if the number's **absolute value is less than a very small tolerance value** (often called epsilon, $\epsilon$).

#### The Core Problem: Why `my_float == 0.0` is Dangerous

Consider this simple calculation: `0.1 + 0.2`. In mathematics, the answer is exactly `0.3`. But in most computer systems, it's not.

```python
>>> 0.1 + 0.2
0.30000000000000004
>>> (0.1 + 0.2) == 0.3
False
```

```py
if __name__ == '__main__':
    a = 0.1 + 0.2
    b = 0.3
    print(a == b)
```

输出: `False`

A calculation that *should* result in zero might instead result in a tiny non-zero value like `5e-17`. A direct comparison `if result == 0.0:` would fail, leading to bugs.

#### The Correct Solution: Using a Tolerance (Epsilon)

The safe way to check for zero is to see if the number is "close enough" to zero.

**The Logic:** `abs(number) < epsilon`

Here's how to implement this in Python, Java, and C++.

---

#### Python

Python's standard library provides a modern, built-in function that handles this perfectly.

##### Best Practice: `math.isclose()`

The `math.isclose()` function (available in Python 3.5+) is the recommended approach. When comparing to zero, it effectively uses an absolute tolerance.

```python
import math

# A calculation that should be zero but isn't
result = (0.1 + 0.2) - 0.3
print(f"Actual value: {result}")  # Output: Actual value: 5.551115123125783e-17

# --- The Wrong Way ---
if result == 0.0:
    print("Wrong way: The number is zero.")
else:
    print("Wrong way: The number is NOT zero.")  # This will be printed

# --- The Correct Way ---
# math.isclose() is designed for this.
# For comparing to 0, the default absolute tolerance (abs_tol) is used.
if math.isclose(result, 0.0):
    print("Correct way: The number is close enough to zero.")  # This will be printed
else:
    print("Correct way: The number is not close to zero.")

# You can also specify your own tolerance
epsilon = 1e-9
if abs(result) < epsilon:
    print("Manual correct way: The number is close enough to zero.")
```

输出如下:

```py
Actual value: 5.551115123125783e-17
Wrong way: The number is NOT zero.
Correct way: The number is not close to zero.
Manual correct way: The number is close enough to zero.
```

#### Java

Java does not have a built-in `isclose` function in its standard `Math` library, so the manual tolerance check is the standard method.

```java
public class CheckFloatZero {
    public static void main(String[] args) {
        // Use 'double' for better precision in general
        double result = (0.1 + 0.2) - 0.3;
        System.out.println("Actual value: " + result); // Output: Actual value: 5.551115123125783E-17

        // --- The Wrong Way ---
        if (result == 0.0) {
            System.out.println("Wrong way: The number is zero.");
        } else {
            System.out.println("Wrong way: The number is NOT zero."); // This will be printed
        }

        // --- The Correct Way ---
        // Define a small tolerance value
        final double EPSILON = 1e-9; 

        if (Math.abs(result) < EPSILON) {
            System.out.println("Correct way: The number is close enough to zero."); // This will be printed
        } else {
            System.out.println("Correct way: The number is not close to zero.");
        }
    }
}
```

*Note: External libraries like Apache Commons Math provide utility functions like `Precision.equals(result, 0.0, EPSILON)` which do the same thing.*

输出如下:

```
Actual value: 5.551115123125783E-17
Wrong way: The number is NOT zero.
Correct way: The number is close enough to zero.
```

#### C++

In C++, the manual tolerance check is also the most common and portable method. You should include the `<cmath>` header for `std::abs`.

```cpp
#include <iostream>
#include <cmath>   // For std::abs
#include <iomanip> // For std::fixed and std::setprecision

int main() {
    // Use 'double' for better precision
    double result = (0.1 + 0.2) - 0.3;
    std::cout << "Actual value: " << std::fixed << std::setprecision(20) << result << std::endl;
    // Output: Actual value: 0.00000000000000005551

    // --- The Wrong Way ---
    if (result == 0.0) {
        std::cout << "Wrong way: The number is zero." << std::endl;
    } else {
        std::cout << "Wrong way: The number is NOT zero." << std::endl; // This will be printed
    }

    // --- The Correct Way ---
    // Define a small tolerance value. 'constexpr' is good practice.
    constexpr double EPSILON = 1e-9;

    if (std::abs(result) < EPSILON) {
        std::cout << "Correct way: The number is close enough to zero." << std::endl; // This will be printed
    } else {
        std::cout << "Correct way: The number is not close to zero." << std::endl;
    }

    return 0;
}
```

输出如下:

```cpp
Actual value: 0.00000000000000005551
Wrong way: The number is NOT zero.
Correct way: The number is close enough to zero.
```

#### Summary and Best Practices

| Language   | Best Practice for Checking if `x` is Zero                                 |
|:---------- |:------------------------------------------------------------------------- |
| **Python** | `math.isclose(x, 0.0)`                                                    |
| **Java**   | `Math.abs(x) < EPSILON` (where `EPSILON` is a small constant like `1e-9`) |
| **C++**    | `std::abs(x) < epsilon` (where `epsilon` is a small constant like `1e-9`) |

**How to choose `epsilon`?**
The value `1e-9` is a common starting point for `double` precision, but the "right" value depends on your application's required accuracy. For most general purposes, it's a safe choice. For financial calculations, you might need a much smaller epsilon, whereas for some physics simulations, a larger one might be acceptable.
