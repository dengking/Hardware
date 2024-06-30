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