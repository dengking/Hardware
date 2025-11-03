# Float point precision error

[softwareengineering-Solutions for floating point rounding errors](https://softwareengineering.stackexchange.com/questions/202843/solutions-for-floating-point-rounding-errors) 

## [softwareengineering-What causes floating point rounding errors?](https://softwareengineering.stackexchange.com/questions/101163/what-causes-floating-point-rounding-errors)

### [A](https://softwareengineering.stackexchange.com/a/101170)

This is because some fractions need a very large (or even infinite) amount of places to be expressed without rounding. This holds true for decimal notation as much as for binary or any other. If you would limit the amount of decimal places to use for your calculations (and avoid making calculations in fraction notation), you would have to round even a simple expression as 1/3 + 1/3. Instead of writing 2/3 as a result you would have to write 0.33333 + 0.33333 = 0.66666 which is not identical to 2/3.

In case of a computer the number of digits is limited by the technical nature of its memory and CPU registers. The binary notation used internally adds some more difficulties. Computers normally can't express numbers in fraction notation, though some programming languages add this ability, which allows those problems to be avoided to a certain degree.

[What Every Computer Scientist Should Know About Floating-Point Arithmetic](http://download.oracle.com/docs/cd/E19957-01/806-3568/ncg_goldberg.html)

### [A](https://softwareengineering.stackexchange.com/a/101197)

In addition to [David Goldberg](http://en.wikipedia.org/wiki/David_E._Goldberg)'s essential **[What Every Computer Scientist Should Know About Floating-Point Arithmetic](http://download.oracle.com/docs/cd/E19957-01/806-3568/ncg_goldberg.html)** (re-published by Sun/Oracle as an appendix to their [Numerical Computation Guide](http://docs.oracle.com/cd/E19957-01/806-3568/ncgTOC.html)), which was mentioned by [thorsten](https://softwareengineering.stackexchange.com/a/101170/22493), the **[ACCU](http://accu.org/)** journal **[Overload](http://accu.org/index.php/aboutus/aboutjournals)** ran an excellent series of articles by [Richard Harris](http://accu.org/index.php/conferences/accu_conference_2011/accu2011_speakers#Richard Harris) about the ***Floating Point Blues***.

## [floating-point-gui-What Every Programmer Should Know About Floating-Point Arithmetic](https://floating-point-gui.de/)

> NOTE:
> 
> 一、这是在 [What causes floating point rounding errors?](https://softwareengineering.stackexchange.com/questions/101163/what-causes-floating-point-rounding-errors) 
> 
> My attempt to clear up the most common confusions: [floating-point-gui.de](http://floating-point-gui.de/) 
> 
> – [Michael Borgwardt](https://softwareengineering.stackexchange.com/users/11982/michael-borgwardt)
> 
>  [Aug 16, 2011 at 11:22](https://softwareengineering.stackexchange.com/questions/101163/what-causes-floating-point-rounding-errors#comment183701_101163)
> 
> 中，发现的

## Round-off error

floating-point imprecision

### wikipedia [Round-off error](https://en.wikipedia.org/wiki/Round-off_error)

### Gemini 2.5 Pro floating-point imprecision

Of course. **Floating-point imprecision** is a fundamental concept in computer science and is the single biggest challenge that robust geometric libraries like JTS are designed to overcome.

Let's break it down from the ground up.

#### The Analogy: Representing 1/3 in Decimal

Imagine you are forced to write down the value of the fraction **1/3** using decimal notation, but you only have a finite number of spaces.

* With 3 spaces, you write: `0.333`
* With 6 spaces, you write: `0.333333`

No matter how many spaces you have, you can *never* write it down perfectly. There will always be a tiny error. `0.333333` is very close to 1/3, but it's not *exactly* 1/3.

`0.333333 * 3 = 0.999999`, not `1`.

This is the exact same problem computers have, but they work in **binary (base-2)** instead of decimal (base-10).

---

#### The Technical Explanation: Binary and Finite Bits

Computers store numbers in binary using a fixed number of bits. The standard for floating-point numbers is **IEEE 754**, which defines formats like `float` (32-bit) and `double` (64-bit).

A `double` stores a number in a form of scientific notation: `sign * mantissa * 2^exponent`.

The problem is that many simple decimal fractions, like `0.1`, become infinitely repeating fractions when converted to binary.

* `0.5` in decimal is `0.1` in binary (exact).
* `0.25` in decimal is `0.01` in binary (exact).
* **`0.1`** in decimal is `0.00011001100110011...` in binary (repeats forever).

Since the computer only has a finite number of bits (52 bits for the mantissa in a `double`), it has to truncate this repeating fraction. The number stored for `0.1` is not exactly `0.1`, but a very close binary approximation.

### The Consequences: Where It All Goes Wrong

This tiny, unavoidable error seems harmless, but it has catastrophic consequences for calculations, especially in geometry.

#### 1. Equality Checks Fail

The most common mistake is comparing two floating-point numbers for exact equality.

```java
double a = 0.1;
double b = 0.2;
double sum = a + b; // Mathematically, this should be 0.3

System.out.println(sum); // Prints 0.30000000000000004
System.out.println(sum == 0.3); // Prints false!
```

Because of the tiny errors in representing `0.1` and `0.2`, their sum is not the exact binary representation of `0.3`.

#### 2. Geometric Predicates Become Unreliable

In computational geometry, algorithms rely on fundamental tests called "predicates." Floating-point imprecision makes them fragile.

* **Point on Line:**
  
  * **Question:** Is point C on the line segment AB?
  * **Problem:** You calculate the position of C relative to the line. Mathematically, the result should be zero if it's on the line. But due to imprecision, you get a tiny non-zero value like $10^{-16}$. A naive algorithm will incorrectly conclude the point is *not* on the line.

* **Line Intersection:**
  
  * **Question:** Do line segment AB and line segment CD intersect?
  * **Problem:** You solve a system of linear equations to find the intersection point. If the lines are nearly parallel or the intersection point is very close to an endpoint, imprecision can cause the algorithm to think they don't intersect when they do, or vice-versa.

* **Polygon Validity:**
  
  * **Question:** Is this a valid polygon? (A key rule is that it must be "closed," meaning the start and end points are identical).
  * **Problem:** A polygon is defined by a list of coordinates, e.g., `[(0,0), (10,10), (0,10), (0,0)]`. If the first and last points were the result of some previous calculation, they might be `(0,0)` and `(1e-15, -1e-16)`. They look identical to a human but are different to the computer, making the polygon technically "not closed."

#### 3. Cumulative Error

When you perform a long sequence of calculations, these tiny errors accumulate. A small error in step 1 can become a larger error in step 2, and by step 10, the final result can be wildly inaccurate and topologically nonsensical.

### How JTS Solves This (The Solution)

This is precisely why JTS has the **`PrecisionModel`**.

Instead of fighting against imprecision with fragile tolerance checks (`if (abs(x) < 0.000001)`), JTS sidesteps the problem entirely.

By using a `PrecisionModel.FIXED`, JTS forces all coordinates onto a grid.

* The point `(1.000000000000001, 1.999999999999999)`
* and the point `(0.999999999999998, 2.000000000000001)`

...might both be snapped to the exact same coordinate `(1.0, 2.0)` (assuming a precision of 1 decimal place).

After snapping, the fragile floating-point comparisons become **exact integer comparisons** on the scaled coordinates. The question "are these two points very close?" becomes "are these two points *identical* after rounding?". This is a much more robust and reliable foundation for complex geometric algorithms.