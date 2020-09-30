# Stored-program computer

stored-program computer是非常重要的思想。



## wikipedia [Stored-program computer](https://en.wikipedia.org/wiki/Stored-program_computer)





## Function and data model

在现代，不断地涌现着新的programming language，不断地涌现着新的programming paradigm，programming technique的提高能够大大提高开发效率，但是无论如何变化，它们最终执行的时候，还是遵循“stored-program computer”的思想，即它们的可执行程序，最终都会被“吃”到memory中，然后由CPU进行执行；为了便于理解各种programming language的run mode，可以将各种programming language的可执行程序（后面简称为program）简化为有下面两部分组成:

| 组成部分 | 说明                           |
| -------- | ------------------------------ |
| function | 函数，包括了普通函数、成员函数 |
| data     | 数据                           |

我们将它简称为Function and data model，显然它是一个统一的模型，建立在programming language和CPU之间。