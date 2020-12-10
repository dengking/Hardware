# 关于本章

本章讨论CPU Speculative execution，我是在学习C-family language的branch prediction optimization technique的时候，发现的这个主题。



## wikipedia [Speculative execution](https://en.wikipedia.org/wiki/Speculative_execution)

> NOTE: "speculative" 的意思是"投机的、思索的、预测的"

**Speculative execution** is an [optimization](https://en.wikipedia.org/wiki/Optimization_(computer_science)) technique where a [computer system](https://en.wikipedia.org/wiki/Computer_system) performs some task that may not be needed. Work is done before it is known whether it is actually needed, so as to prevent a delay that would have to be incurred by doing the work after it is known that it is needed. If it turns out the work was not needed after all, most changes made by the work are reverted and the results are ignored.

The objective is to provide more [concurrency](https://en.wikipedia.org/wiki/Concurrency_(computer_science)) if extra [resources](https://en.wikipedia.org/wiki/Resource_(computer_science)) are available. 

> NOTE: 此处对它的目的是总结是: provide more concurrency

This approach is employed in a variety of areas, including: 

1 [branch prediction](https://en.wikipedia.org/wiki/Branch_predictor) in [pipelined](https://en.wikipedia.org/wiki/Instruction_pipeline) [processors](https://en.wikipedia.org/wiki/CPU), 

2 value prediction for exploiting value locality,[[1\]](https://en.wikipedia.org/wiki/Speculative_execution#cite_note-vpvls-1) 

3 prefetching [memory](https://en.wikipedia.org/wiki/Instruction_prefetch) and [files](https://en.wikipedia.org/wiki/File_system), and 

4 [optimistic concurrency control](https://en.wikipedia.org/wiki/Optimistic_concurrency_control) in [database systems](https://en.wikipedia.org/wiki/Relational_database_management_system).[[2\]](https://en.wikipedia.org/wiki/Speculative_execution#cite_note-Lampson-2)[[3\]](https://en.wikipedia.org/wiki/Speculative_execution#cite_note-DivisionRaghavan1998-3)[[4\]](https://en.wikipedia.org/wiki/Speculative_execution#cite_note-4)

5 [Speculative multithreading](https://en.wikipedia.org/wiki/Speculative_multithreading) is a special case of speculative execution.

> NOTE: Speculative execution是一种非常重要的optimization technique，它的目的是: "provide more concurrency"(参见上面的描述)。
>
> 它在computer science的各个field中都有着应用。
>
> 这体现了: "a technique/thought can be used in a variety of areas"，即 "同一个思想，在不同领域的应用"，从下面的 "Variants" 章节可以看出，在不同领域对这个思想进行运用的时候，往往会进行一定的调整，因此会产生新的concept。

### Overview

> NOTE: 原文这一段所描述的是 Branch predictor

### Variants

*Speculative computation* was a related earlier concept.[[6\]](https://en.wikipedia.org/wiki/Speculative_execution#cite_note-6)

#### Eager execution

*See also:* [Eager evaluation](https://en.wikipedia.org/wiki/Eager_evaluation)

#### Predictive execution

See also: [Pipeline (computing)](https://en.wikipedia.org/wiki/Pipeline_(computing))

Main article: [Branch predictor](https://en.wikipedia.org/wiki/Branch_predictor)

Common forms of this include [branch predictors](https://en.wikipedia.org/wiki/Branch_predictor) and [memory dependence prediction](https://en.wikipedia.org/wiki/Memory_dependence_prediction). A generalized form is sometimes referred to as value prediction.[[1\]](https://en.wikipedia.org/wiki/Speculative_execution#cite_note-vpvls-1)[[8\]](https://en.wikipedia.org/wiki/Speculative_execution#cite_note-8)

> NOTE: 在后面章节会进行描述

### Related concepts

#### Lazy execution

*Main article:* [Lazy evaluation](https://en.wikipedia.org/wiki/Lazy_evaluation)

Lazy execution is the opposite of eager execution, and does not involve speculation.