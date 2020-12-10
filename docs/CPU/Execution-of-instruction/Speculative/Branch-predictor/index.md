# Branch predictor

在C family programming，可以开启这个optimization，参见工程programming-language的`C++\Language-reference\Attribute\Likely-and-unlikely`章节。

## computerhope [Branch prediction](https://www.computerhope.com/jargon/b/branch-prediction.htm)

> NOTE: 解释得比较好

**Branch prediction** is a technique used in [CPU](https://www.computerhope.com/jargon/c/cpu.htm) design that attempts to guess the outcome of a [conditional operation](https://www.computerhope.com/jargon/c/contstat.htm) and prepare for the most likely result. A [digital](https://www.computerhope.com/jargon/d/digital.htm) [circuit](https://www.computerhope.com/jargon/c/circuit.htm) that performs this operation is known as a **branch predictor**. It is an important component of modern CPU [architectures](https://www.computerhope.com/jargon/a/architec.htm), such as the [x86](https://www.computerhope.com/jargon/x/x86.htm).

### How does it work?

When a conditional operation such as an [if…else](https://www.computerhope.com/jargon/i/ifelse.htm) statement needs to be processed, the branch predictor "speculates" which condition most likely will be met. It then [executes](https://www.computerhope.com/jargon/e/execute.htm) the operations required by the most likely result ahead of time. This way, they're already complete if and when the guess was correct. At [runtime](https://www.computerhope.com/jargon/r/runtime.htm), if the guess turns out not to be correct, the CPU executes the other branch of operation, incurring a slight delay. But if the guess was correct, speed is significantly increased.

The first time a conditional operation is seen, the branch predictor does not have much information to use as the basis of a guess. But the more frequently the same operation is used, the more accurate its guess can become.

## wikipedia [Branch predictor](https://en.wikipedia.org/wiki/Branch_predictor)

In [computer architecture](https://en.wikipedia.org/wiki/Computer_architecture), a **branch predictor**[[1\]](https://en.wikipedia.org/wiki/Branch_predictor#cite_note-dbp-class-report-1)[[2\]](https://en.wikipedia.org/wiki/Branch_predictor#cite_note-schemes-and-performances-2)[[3\]](https://en.wikipedia.org/wiki/Branch_predictor#cite_note-3)[[4\]](https://en.wikipedia.org/wiki/Branch_predictor#cite_note-4)[[5\]](https://en.wikipedia.org/wiki/Branch_predictor#cite_note-5) is a [digital circuit](https://en.wikipedia.org/wiki/Digital_electronics) that tries to guess which way a [branch](https://en.wikipedia.org/wiki/Branch_(computer_science)) (e.g., an [if–then–else structure](https://en.wikipedia.org/wiki/Conditional_(programming))) will go before this is known definitively. The purpose of the branch predictor is to improve the flow in the [instruction pipeline](https://en.wikipedia.org/wiki/Instruction_pipeline). Branch predictors play a critical role in achieving high effective [performance](https://en.wikipedia.org/wiki/Computer_performance) in many modern [pipelined](https://en.wikipedia.org/wiki/Pipeline_(computing)) [microprocessor](https://en.wikipedia.org/wiki/Microprocessor) architectures[[6\]](https://en.wikipedia.org/wiki/Branch_predictor#cite_note-BPdynSurvey-6) such as [x86](https://en.wikipedia.org/wiki/X86).