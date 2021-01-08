# Multi-core processor

# wikipedia [Multi-core processor](https://en.wikipedia.org/wiki/Multi-core_processor)

A **multi-core processor** is a single [computing](https://en.wikipedia.org/wiki/Computing) component with two or more independent [processing units](https://en.wikipedia.org/wiki/Central_processing_unit) called cores, which read and execute [program instructions](https://en.wikipedia.org/wiki/Instruction_set).[[1\]](https://en.wikipedia.org/wiki/Multi-core_processor#cite_note-1) The instructions are ordinary [CPU instructions](https://en.wikipedia.org/wiki/Instruction_set) (such as add, move data, and branch) but the single processor can run multiple instructions on separate cores at the same time, increasing overall speed for programs amenable to [parallel computing](https://en.wikipedia.org/wiki/Parallel_computing).[[2\]](https://en.wikipedia.org/wiki/Multi-core_processor#cite_note-2) Manufacturers typically integrate the cores onto a single [integrated circuit](https://en.wikipedia.org/wiki/Integrated_circuit) [die](https://en.wikipedia.org/wiki/Die_(integrated_circuit)) (known as a chip multiprocessor or CMP) or onto multiple dies（管芯） in a single [chip package](https://en.wikipedia.org/wiki/Chip_carrier). The microprocessors currently used in almost all personal computers are multi-core.



A multi-core processor implements [multiprocessing](https://en.wikipedia.org/wiki/Multiprocessing) in a single physical package. Designers may couple cores in a multi-core device tightly or loosely. For example, cores may or may not share [caches](https://en.wikipedia.org/wiki/CPU_cache), and they may implement [message passing](https://en.wikipedia.org/wiki/Message_passing) or [shared-memory](https://en.wikipedia.org/wiki/Shared_memory) inter-core communication methods. Common [network topologies](https://en.wikipedia.org/wiki/Network_topology) to interconnect cores include [bus](https://en.wikipedia.org/wiki/Bus_network), [ring](https://en.wikipedia.org/wiki/Ring_network), two-dimensional [mesh](https://en.wikipedia.org/wiki/Mesh_networking), and [crossbar](https://en.wikipedia.org/wiki/Crossbar_switch). Homogeneous multi-core systems include only identical cores; [heterogeneous](https://en.wikipedia.org/wiki/Heterogeneous_computing) multi-core systems have cores that are not identical (e.g. [big.LITTLE](https://en.wikipedia.org/wiki/ARM_big.LITTLE) have heterogeneous cores that share the same instruction set, while [AMD Accelerated Processing Units](https://en.wikipedia.org/wiki/AMD_Accelerated_Processing_Unit) have cores that don't even share the same instruction set). Just as with single-processor systems, cores in multi-core systems may implement architectures such as [VLIW](https://en.wikipedia.org/wiki/Very_long_instruction_word), [superscalar](https://en.wikipedia.org/wiki/Superscalar_processor), [vector](https://en.wikipedia.org/wiki/Vector_processor), or [multithreading](https://en.wikipedia.org/wiki/Multithreading_(computer_architecture)).

Multi-core processors are widely used across many application domains, including [general-purpose](https://en.wikipedia.org/wiki/Computer), [embedded](https://en.wikipedia.org/wiki/Embedded_system), [network](https://en.wikipedia.org/wiki/Network_processor), [digital signal processing](https://en.wikipedia.org/wiki/Digital_signal_processing) (DSP), and [graphics](https://en.wikipedia.org/wiki/Graphics_processing_unit) (GPU).

**The improvement in performance gained by the use of a multi-core processor depends very much on the [software](https://en.wikipedia.org/wiki/Software) algorithms used and their implementation**. In particular, possible gains are limited by the fraction of the software that can [run in parallel](https://en.wikipedia.org/wiki/Parallel_computing) simultaneously on multiple cores; this effect is described by [Amdahl's law](https://en.wikipedia.org/wiki/Amdahl%27s_law). In the best case, so-called [embarrassingly parallel](https://en.wikipedia.org/wiki/Embarrassingly_parallel) problems may realize **speedup factors**（加速倍数） near the number of cores, or even more if the problem is split up enough to fit within each core's cache(s), avoiding use of much slower **main-system memory**. Most applications, however, are not accelerated so much unless **programmers** invest a prohibitive amount of effort in re-factoring（重构） the whole problem.[[3\]](https://en.wikipedia.org/wiki/Multi-core_processor#cite_note-3) The **parallelization** of software is a significant ongoing topic of research.

## Software effects

Managing [concurrency](https://en.wikipedia.org/wiki/Concurrent_computing) acquires a central role in developing parallel applications. The basic steps in designing parallel applications are:

- Partitioning 划分

  The **partitioning stage** of a design is intended to expose opportunities for parallel execution. Hence, the focus is on defining a large number of small tasks in order to yield what is termed a **fine-grained decomposition**（细粒度分解） of a problem.

- Communication 

  The tasks generated by a partition are intended to execute concurrently but cannot, in general, execute independently. The computation to be performed in one task will typically require data associated with another task. Data must then be transferred between tasks so as to allow computation to proceed. This **information flow** is specified in the communication phase of a design.

- Agglomeration  集聚

  In the third stage, development moves from the abstract toward the concrete. Developers revisit decisions made in the partitioning and communication phases with a view to obtaining an algorithm that will execute efficiently on some class of parallel computer. In particular, developers consider whether it is useful to combine, or agglomerate, tasks identified by the partitioning phase, so as to provide a smaller number of tasks, each of greater size. They also determine whether it is worthwhile to replicate data and computation.

- Mapping 

  In the fourth and final stage of the design of parallel algorithms, the developers specify where each task is to execute. This mapping problem does not arise on uniprocessors or on shared-memory computers that provide automatic task scheduling.

On the other hand, on the [server side](https://en.wikipedia.org/wiki/Server-side), multi-core processors are ideal because they allow many users to connect to a site simultaneously and have independent [threads](https://en.wikipedia.org/wiki/Thread_(computer_science)) of execution. This allows for Web servers and application servers that have much better [throughput](https://en.wikipedia.org/wiki/Throughput).


# 20181130

https://unix.stackexchange.com/questions/218074/how-to-know-number-of-cores-of-a-system-in-linux

