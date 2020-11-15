# Out-of-order execution

简单而又，OoOE是CPU为了performance，不按照In-order execution，即[instruction cycle](https://infogalactic.com/info/Instruction_cycle)，而是采取特殊的执行方式。

## Wikipedia [Out-of-order execution](https://infogalactic.com/info/Out-of-order_execution)

In [computer engineering](https://infogalactic.com/info/Computer_engineering), **out-of-order execution** (or more formally **dynamic execution**), is a [paradigm](https://infogalactic.com/info/Paradigm) used in most high-performance [microprocessors](https://infogalactic.com/info/Microprocessor) to make use of [instruction cycles](https://infogalactic.com/info/Instruction_cycle) that would otherwise be wasted by a certain type of costly delay. In this paradigm, a processor executes instructions in an order governed by the availability of input data, rather than by their original order in a program.[[1\]](https://infogalactic.com/info/Out-of-order_execution#cite_note-1) In doing so, the processor can avoid being idle while waiting for the preceding instruction to complete to retrieve data for the next instruction in a program, processing instead the next instructions which are able to run immediately and independently.[[2\]](https://infogalactic.com/info/Out-of-order_execution#cite_note-2) It can be viewed as a hardware based [dynamic recompilation](https://infogalactic.com/info/Dynamic_recompilation) or [just-in-time compilation](https://infogalactic.com/info/Just-in-time_compilation) (JIT) to improve [instruction scheduling](https://infogalactic.com/info/Instruction_scheduling).

> NOTE: CPU的out-of-order execution是处于performance考虑的

### Basic concept

#### In-order processors

*Main article:* [Instruction cycle](https://infogalactic.com/info/Instruction_cycle)

#### Out-of-order processors

> NOTE: 比较复杂

The key concept of OoOE processing is to allow the processor to avoid a class of stalls that occur when the data needed to perform an operation are unavailable.

The benefit of OoOE processing grows as the [instruction pipeline](https://infogalactic.com/info/Instruction_pipeline) deepens and the speed difference between [main memory](https://infogalactic.com/info/Main_memory) (or [cache memory](https://infogalactic.com/info/Cache_memory)) and the processor widens. On modern machines, the processor runs many times faster than the memory, so during the time an in-order processor spends waiting for data to arrive, it could have processed a large number of instructions.