# Memory disambiguation

"disambiguation"的含义是"消除歧义"。

Memory disambiguation也是由out-of-order-execution引发的。



## wikipedia [Memory disambiguation](https://en.wikipedia.org/wiki/Memory_disambiguation)

**Memory disambiguation** is a set of techniques employed by high-performance [out-of-order execution](https://en.wikipedia.org/wiki/Out-of-order_execution) [microprocessors](https://en.wikipedia.org/wiki/Microprocessor) that execute [memory](https://en.wikipedia.org/wiki/Primary_storage) access [instructions](https://en.wikipedia.org/wiki/Instruction_(computer_science)) (loads and stores) out of program order. The mechanisms for performing memory disambiguation, implemented using [digital logic](https://en.wikipedia.org/wiki/Digital_circuit) inside the microprocessor core, detect true dependencies between memory operations at execution time and allow the processor to recover when a dependence has been violated. They also eliminate spurious memory dependencies and allow for greater [instruction-level parallelism](https://en.wikipedia.org/wiki/Instruction-level_parallelism) by allowing safe out-of-order execution of loads and stores.