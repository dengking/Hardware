# Function and data model

在现代，不断地涌现着新的programming language，不断地涌现着新的programming paradigm（比如从procedural到OOP），programming technique的提高能够大大提高开发效率，但是无论如何变化，它们最终执行的时候，还是遵循“stored-program computer”的思想，即它们的可执行程序，最终都会被“吃”到RAM中，然后由CPU进行执行；为了便于理解各种programming language的run mode，可以将各种programming language的可执行程序（后面简称为program）简化为有下面两部分组成:

| 组成部分 | 说明 | hardware                                                    |
| -------- | ---- | ----------------------------------------------------------- |
| function | 函数 | 从汇编的角度来看，函数就是一堆指令，因此对应的是instruction |
| data     | 数据 | 对应的是memory                                              |

我们将它简称为**Function and data model**，显然它是一个统一的模型，建立在programming language和CPU之间。

## Software and hardware

**Function and data model**连接software和hardware的uniform model，关于Software and hardware的讨论，参见文章《Software-and-hardware》

---

下面结合一些具体问题，来对它进行描述。

## Process VMA

### wikipedia [Memory address # Address space in application programming](https://en.wikipedia.org/wiki/Memory_address#Address_space_in_application_programming) 

> NOTE: 将process的VMA分为两部分:
>
> 1) **[Machine code](https://en.wikipedia.org/wiki/Machine_code)** / instruction，对应的是function
>
> 2) **[Data](https://en.wikipedia.org/wiki/Data_(computing))**
>
> 显然，最终我们的process（run-time概念）可以看做是有上述两部分组成，显然这样的抽象有助于我们对run mode的理解；

Each memory location in a [stored-program computer](https://en.wikipedia.org/wiki/Stored-program_computer) holds a [binary number](https://en.wikipedia.org/wiki/Binary_number) or [decimal number](https://en.wikipedia.org/wiki/Decimal_number) *of some sort*. Its interpretation, as data of some [data type](https://en.wikipedia.org/wiki/Data_type) or as an instruction, and use are determined by the [instructions](https://en.wikipedia.org/wiki/Instruction_(computer_science)) which retrieve and manipulate it.

In modern [multitasking](https://en.wikipedia.org/wiki/Computer_multitasking) environment, an [application](https://en.wikipedia.org/wiki/Application_program) [process](https://en.wikipedia.org/wiki/Process_(computing)) usually has in its address space (or spaces) chunks of memory of following types:

1) [Machine code](https://en.wikipedia.org/wiki/Machine_code), including:

- program's own code (historically known as *[code segment](https://en.wikipedia.org/wiki/Code_segment)* or *text segment*);
- [shared libraries](https://en.wikipedia.org/wiki/Shared_libraries).

2) [Data](https://en.wikipedia.org/wiki/Data_(computing)), including:

- initialized data ([data segment](https://en.wikipedia.org/wiki/Data_segment));
- [uninitialized (but allocated)](https://en.wikipedia.org/wiki/.bss) variables;
- [run-time stack](https://en.wikipedia.org/wiki/Run-time_stack);
- [heap](https://en.wikipedia.org/wiki/Heap_(programming));
- [shared memory](https://en.wikipedia.org/wiki/Shared_memory_(interprocess_communication)) and [memory mapped files](https://en.wikipedia.org/wiki/Memory_mapped_file).

Some parts of address space may be not mapped at all.

## OOP

越来越多的programming language支持OOP，那么我们不仅要问：Function and data model能否描述使用 OOP language编写的的program呢？答案是: 可以的。分析如下: 

OOP只是一种programming paradigm，不同的programming的实现方式是不同的，但是不管如何实现，它的member method本质上是function，member variable本质上是data，依然可以使用Function and data model来进行描述。因此相对于传统的procedure programming language而言，很多概念都需要向OOP扩展: 

| 组成部分 | 说明                                    |
| -------- | --------------------------------------- |
| function | 包括: <br>- 普通函数<br>- 成员函数;<br> |
| data     | 包括: <br>- 普通data <br>- member data  |



## Pointer

由于它们都位于RAM中，因此都有着address，因此可以通过pointer来**访问**/**引用**:

| pointer             | explanation                                                  |
| ------------------- | ------------------------------------------------------------ |
| pointer to function | [Stored-program computer](https://en.wikipedia.org/wiki/Stored-program_computer) 启发了我们 stores [program instructions](https://en.wikipedia.org/wiki/Instruction_(computer_science)) in electronic memory. 所以我们的所编写的function，最终也是会存入到 RAM中的，因此，它们就和数据一样，可以通过pointer对其进行access；所以这就是function pointer的本质所在； |
| pointer to data     |                                                              |

## Memory access

在 `CPU-memory-access` 章节进行了介绍。



### Operation on memory

1. read
2. write

