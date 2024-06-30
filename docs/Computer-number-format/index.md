# Computer number format/representation

一、从最最基础的、最最底层的开始思考: computer以怎样的方式来表示、存储number的。它是理解后续很多内容的前提。

number的分类:

法一:

1、integer

2、float-point number

法二: 根据 number的signedness属性 

值域、overflow、underflow



## wikipedia [Computer number format](https://en.wikipedia.org/wiki/Computer_number_format)

> NOTE: 
>
> 1、无论是什么类型的number，在CPU中，都是bit；不同的类型number，CPU会采用不同的instruction来对它们进行处理，因此它们会被送到不同的运算单元进行计算，这些运算单元会按照固定的方式来对这些bit进行解释，这是典型的interpretation model-type determine everthing

### Binary number representation



#### Converting between bases

> NOTE:
>
> 1、所谓的base，指的是进制

*Main article:* [Positional notation (base conversion)](https://en.wikipedia.org/wiki/Positional_notation#Base_conversion)

> NOTE: 
>
> 1、"notation"可以翻译为"计数法"

Each of these number systems is a positional system, but while decimal weights are powers of 10, the octal weights are powers of 8 and the hexadecimal weights are powers of 16. 

> NOTE: 
>
> 1、上面提及了 [Positional notation (base conversion)](https://en.wikipedia.org/wiki/Positional_notation#Base_conversion) 的概念

### Representing fractions in binary

> NOTE:
>
> 1、"fraction"的意思是"小数"

#### Fixed-point numbers

> NOTE: 
>
> 1、定点数
>
> 2、这种方式目前不是主流

#### Floating-point numbers

> NOTE: 这部分内容移到了`Floating-point-number`章节