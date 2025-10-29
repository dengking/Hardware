# [docs.oracle-What Every Computer Scientist Should Know About Floating-Point Arithmetic](https://docs.oracle.com/cd/E19957-01/806-3568/ncg_goldberg.html)

> NOTE:
> 
> 一、在 [softwareengineering-What causes floating point rounding errors?](https://softwareengineering.stackexchange.com/questions/101163/what-causes-floating-point-rounding-errors) # [A](https://softwareengineering.stackexchange.com/a/101170) 中发现的这篇文章

---

**Note –** This appendix is an edited reprint of the paper *What Every Computer Scientist Should Know About Floating-Point Arithmetic*, by David Goldberg, published in the March, 1991 issue of Computing Surveys. Copyright 1991, Association for Computing Machinery, Inc., reprinted by permission.

---

> 翻译: 本附录是戴维・戈德堡（David Goldberg）所著论文《每位计算机科学家都应了解的浮点数运算知识》（*What Every Computer Scientist Should Know About Floating-Point Arithmetic*）的修订重印版。该论文最初发表于 1991 年 3 月的《计算综述》（*Computing Surveys*）期刊，版权归 1991 年计算机协会（Association for Computing Machinery, Inc.）所有，经许可重印。

## Abstract

Floating-point arithmetic is considered an esoteric(深奥的) subject by many people. This is rather surprising because floating-point is ubiquitous in computer systems. Almost every language has a floating-point datatype; computers from PCs to supercomputers have floating-point accelerators; most compilers will be called upon to compile floating-point algorithms from time to time; and virtually every operating system must respond to floating-point exceptions such as overflow. This paper presents a tutorial on those aspects of floating-point that have a direct impact on designers of computer systems. It begins with background on floating-point representation and [**rounding error**](https://en.wikipedia.org/wiki/Round-off_error), continues with a discussion of the IEEE floating-point standard, and concludes with numerous examples of how computer builders can better support floating-point.

> 翻译: 许多人将浮点数运算视为一门深奥的学科。这相当令人意外，因为浮点数运算在计算机系统中其实无处不在。

- Categories and Subject Descriptors: (Primary) C.0 [Computer Systems Organization]: General -- *instruction set design*; D.3.4 [Programming Languages]: Processors -- *compilers, optimization*; G.1.0 [Numerical Analysis]: General -- *computer arithmetic, error analysis, numerical algorithms* (Secondary)

- D.2.1 [Software Engineering]: Requirements/Specifications -- *languages*; D.3.4 Programming Languages]: Formal Definitions and Theory -- *semantics*; D.4.1 Operating Systems]: Process Management -- *synchronization*.

General Terms: Algorithms, Design, Languages

Additional Key Words and Phrases: [Denormalized number](https://en.wikipedia.org/wiki/Subnormal_number)(非规格化), exception, floating-point, floating-point standard, gradual underflow(渐进下溢), [guard digit](https://en.wikipedia.org/wiki/Guard_digit), NaN, overflow, [relative error](https://en.wikipedia.org/wiki/Approximation_error), rounding error, rounding mode, [ulp](https://en.wikipedia.org/wiki/Unit_in_the_last_place)(最后一位的单位), underflow.

## Introduction

Builders of computer systems often need information about floating-point arithmetic. There are, however, remarkably few sources of detailed information about it. One of the few books on the subject, *Floating-Point Computation* by Pat Sterbenz, is long out of print. This paper is a tutorial on those aspects of floating-point arithmetic (*floating-point* hereafter) that have a direct connection to systems building. It consists of three loosely connected parts:

- The first section, [Rounding Error](https://docs.oracle.com/cd/E19957-01/806-3568/ncg_goldberg.html#680), discusses the implications of using different rounding strategies for the basic operations of addition, subtraction, multiplication and division. It also contains background information on the two methods of measuring rounding error:
  
  - ulps 
  
  - `relative error`. 

- The second part discusses the IEEE floating-point standard, which is becoming rapidly accepted by commercial hardware manufacturers. Included in the IEEE standard is the rounding method for basic operations. The discussion of the standard draws on(借鉴) the material in the section [Rounding Error](https://docs.oracle.com/cd/E19957-01/806-3568/ncg_goldberg.html#680). 

- The third part discusses the connections between floating-point and the design of various aspects of computer systems. Topics include 
  
  - instruction set design
  
  - optimizing compilers
  
  - exception handling.

I have tried to avoid making statements about floating-point without also giving reasons why the statements are true, especially since the justifications involve nothing more complicated than elementary calculus. Those explanations that are not central to the main argument have been grouped into a section called "The Details," so that they can be skipped if desired. In particular, the proofs of many of the theorems appear in this section. The end of each proof is marked with the z symbol. When a proof is not included, the z appears immediately following the statement of the theorem.

> 翻译: 我一直努力避免在阐述浮点数相关观点时，不给出这些观点成立的理由 —— 尤其是因为这些理由的推导，只需要基础微积分知识，并不涉及更复杂的内容。那些并非核心论证关键的解释，已被整合到一个名为 “细节补充”（The Details）的章节中，读者若有需要可略过不读。具体而言，许多定理的证明过程都收录在该章节内，每段证明的结尾均以 “z” 符号标示。若某个定理未附带证明过程，则会在定理表述结束后直接标注 “z” 符号。

## Rounding Error

Squeezing infinitely many **real numbers** into a finite number of bits requires an approximate representation. Although there are infinitely many **integers**, in most programs the result of integer computations can be stored in 32 bits. In contrast, given any fixed number of bits, most calculations with real numbers will produce quantities that cannot be exactly represented using that many bits. Therefore the result of a floating-point calculation must often be rounded in order to fit back into its finite representation. This rounding error is the characteristic feature of floating-point computation. The section [Relative Error and Ulps](https://docs.oracle.com/cd/E19957-01/806-3568/ncg_goldberg.html#689) describes how it is measured.

> 翻译: 将无限多个实数压缩到有限的比特位中，需要采用近似表示。尽管整数的数量也是无限的，但在大多数程序中，整数运算的结果可以用 32 个比特位存储。与之相反，给定任意固定数量的比特位，大多数实数运算产生的结果，都无法用该数量的比特位精确表示。因此，浮点数运算的结果往往需要进行舍入，才能适配回其有限的表示形式。这种舍入误差是浮点数运算的标志性特征，其度量方式在 “相对误差与最后一位单位（Relative Error and Ulps）” 章节中有所说明。

Since most floating-point calculations have rounding error anyway, does it matter if the basic arithmetic operations introduce a little bit more rounding error than necessary? That question is a main theme throughout this section. The section [Guard Digits](https://docs.oracle.com/cd/E19957-01/806-3568/ncg_goldberg.html#693) discusses *guard* digits, a means of reducing the error when subtracting(减法) two nearby numbers. **Guard digits** were considered sufficiently important by IBM that in 1968 it added a guard digit to the double precision format in the System/360 architecture (single precision already had a guard digit), and retrofitted all existing machines in the field. Two examples are given to illustrate the utility of guard digits.

> 翻译: 既然大多数浮点数计算总归会存在舍入误差，那么基本算术运算引入的舍入误差是否比必要值多一点，还重要吗？这个问题是本节的核心主题。“保护位”（Guard Digits）章节探讨了保护位 —— 一种在对两个相近数值做减法时减少误差的方法。保护位的重要性得到了 IBM 的高度认可，该公司在 1968 年为 System/360 架构的双精度格式增设了一个保护位（单精度格式当时已具备保护位），并对当时市面上所有在用的该架构机器进行了翻新改造。本节还通过两个示例，阐明了保护位的实用价值。

The IEEE standard goes further than just requiring the use of a guard digit. It gives an algorithm for addition, subtraction, multiplication, division and square root, and requires that implementations produce the same result as that algorithm. Thus, when a program is moved from one machine to another, the results of the basic operations will be the same in every bit if both machines support the IEEE standard. This greatly simplifies the porting of programs. Other uses of this precise specification are given in [Exactly Rounded Operations](https://docs.oracle.com/cd/E19957-01/806-3568/ncg_goldberg.html#704).
