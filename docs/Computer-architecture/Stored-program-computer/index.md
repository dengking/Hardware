# Stored-program computer

stored-program computer是非常重要的思想。



## wikipedia [Stored-program computer](https://en.wikipedia.org/wiki/Stored-program_computer)

A **stored-program computer** is one which stores [program instructions](https://infogalactic.com/info/Instruction_(computer_science)) in electronic memory.[[1\]](https://infogalactic.com/info/Stored-program_computer#cite_note-1) Often the definition is extended with the requirement that the treatment of programs and data in memory be interchangeable or uniform.[[2\]](https://infogalactic.com/info/Stored-program_computer#cite_note-GilreathLaplante2003-2)[[3\]](https://infogalactic.com/info/Stored-program_computer#cite_note-Reilly2003-3)[[4\]](https://infogalactic.com/info/Stored-program_computer#cite_note-POCA-4)

A computer with a [Von Neumann architecture](https://infogalactic.com/info/Von_Neumann_architecture) stores program data and instruction data in the same memory; a computer with a [Harvard architecture](https://infogalactic.com/info/Harvard_architecture) has separate memories for storing program and data.[[5\]](https://infogalactic.com/info/Stored-program_computer#cite_note-Page2009-5)[[6\]](https://infogalactic.com/info/Stored-program_computer#cite_note-Balch2003-6)

> NOTE: von neumann architecture 和 Harvard architecture的差异



The stored-program computer idea can be traced back to the 1936 theoretical concept of a [universal Turing machine](https://infogalactic.com/info/Universal_Turing_machine).[[11\]](https://infogalactic.com/info/Stored-program_computer#cite_note-Copeland2006-11) Von Neumann was aware of this paper, and he impressed it on his collaborators as well.[[12\]](https://infogalactic.com/info/Stored-program_computer#cite_note-Teuscher2004-12)

> NOTE: 关于谁首先提出这个概念，历史学家追溯到了Turing



## wikipedia [Universal Turing machine § Stored-program computer](https://en.wikipedia.org/wiki/Universal_Turing_machine#Stored-program_computer) 



## Function and data model

在现代，不断地涌现着新的programming language，不断地涌现着新的programming paradigm（比如从procedural到OOP），programming technique的提高能够大大提高开发效率，但是无论如何变化，它们最终执行的时候，还是遵循“stored-program computer”的思想，即它们的可执行程序，最终都会被“吃”到RAM中，然后由CPU进行执行；为了便于理解各种programming language的run mode，可以将各种programming language的可执行程序（后面简称为program）简化为有下面两部分组成:

| 组成部分 | 说明                                         |
| -------- | -------------------------------------------- |
| function | 函数<br>从汇编的角度来看，函数就是一堆指令； |
| data     | 数据                                         |

我们将它简称为Function and data model，显然它是一个统一的模型，建立在programming language和CPU之间。下面结合一些具体问题，来对它进行描述。

### OOP

越来越多的programming language支持OOP，那么我们不仅要问：Function and data model能否描述使用 OOP language编写的的program呢？答案是: 可以的。分析如下: 

OOP只是一种programming paradigm，不同的programming的实现方式是不同的，但是不管如何实现，它的member method本质上是function，member variable本质上是data，依然可以使用Function and data model来进行描述。因此相对于传统的procedure programming language而言，很多概念都需要向OOP扩展: 

| 组成部分 | 说明                                    |
| -------- | --------------------------------------- |
| function | 包括: <br>- 普通函数<br>- 成员函数;<br> |
| data     | 包括: <br>- 普通data <br>- member data  |



### Process

显然，最终我们的process（run-time概念）可以看做是有上述两部分组成，显然这样的抽象有助于我们对run mode的理解；

### Pointer

由于它们都位于RAM中，因此都有着address，因此可以通过pointer来**访问**/**引用**:

| pointer             | explanation                                                  |
| ------------------- | ------------------------------------------------------------ |
| pointer to function | [Stored-program computer](https://en.wikipedia.org/wiki/Stored-program_computer) 启发了我们 stores [program instructions](https://en.wikipedia.org/wiki/Instruction_(computer_science)) in electronic memory. 所以我们的所编写的function，最终也是会存入到 RAM中的，因此，它们就和数据一样，可以通过pointer对其进行access；所以这就是function pointer的本质所在； |
| pointer to data     |                                                              |

