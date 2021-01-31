# kernel [LINUX KERNEL MEMORY BARRIERS](https://www.kernel.org/doc/Documentation/memory-barriers.txt)

> NOTE: 这是非常好的内容，在 preshing [Memory Barriers Are Like Source Control Operations](https://preshing.com/20120710/memory-barriers-are-like-source-control-operations/) 的最后也推荐了这篇文章。

## DISCLAIMER



## CONTENTS

###  (*) Abstract memory access model.

     - Device operations.
     - Guarantees.

###  (*) What are memory barriers?

     - Varieties of memory barrier.
     - What may not be assumed about memory barriers?
     - Data dependency barriers (historical).
     - Control dependencies.
     - SMP barrier pairing.
     - Examples of memory barrier sequences.
     - Read memory barriers vs load speculation.
     - Multicopy atomicity.

###  (*) Explicit kernel barriers.

     - Compiler barrier.
     - CPU memory barriers.

###  (*) Implicit kernel memory barriers.

     - Lock acquisition functions.
     - Interrupt disabling functions.
     - Sleep and wake-up functions.
     - Miscellaneous functions.

###  (*) Inter-CPU acquiring barrier effects.

     - Acquires vs memory accesses.

###  (*) Where are memory barriers needed?

     - Interprocessor interaction.
     - Atomic operations.
     - Accessing devices.
     - Interrupts.

###  (*) Kernel I/O barrier effects.

###  (*) Assumed minimum execution ordering model.

###  (*) The effects of the cpu cache.

     - Cache coherency.
     - Cache coherency vs DMA.
     - Cache coherency vs MMIO.

###  (*) The things CPUs get up to.

     - And then there's the Alpha.
     - Virtual Machine Guests.

###  (*) Example uses.

     - Circular buffers.

###  (*) References.