# Fetch-and-add

## wikipedia [Fetch-and-add](https://en.wikipedia.org/wiki/Fetch-and-add)

In [computer science](https://en.wikipedia.org/wiki/Computer_science), the **fetch-and-add** [CPU](https://en.wikipedia.org/wiki/Central_processing_unit) instruction (FAA) [atomically](https://en.wikipedia.org/wiki/Atomic_(computer_science)) increments the contents of a memory location by a specified value.

That is, fetch-and-add performs the operation

> increment the value at address *x* by *a*, where *x* is a memory location and *a* is some value, and return the original value at *x*

in such a way that if this operation is executed by one process in a [concurrent](https://en.wikipedia.org/wiki/Concurrent_computing) system, no other process will ever see an intermediate result.

Fetch-and-add can be used to implement [concurrency control](https://en.wikipedia.org/wiki/Concurrency_control) structures such as [mutex locks](https://en.wikipedia.org/wiki/Mutual_exclusion) and [semaphores](https://en.wikipedia.org/wiki/Semaphore_(programming)).



### Overview

The motivation for having an atomic fetch-and-add is that operations that appear in programming languages as

x = x + a

are not safe in a concurrent system, where multiple [processes](https://en.wikipedia.org/wiki/Process_(computing)) or [threads](https://en.wikipedia.org/wiki/Thread_(computing)) are running concurrently (either in a [multi-processor](https://en.wikipedia.org/wiki/Multi-processor) system, or [preemptively](https://en.wikipedia.org/wiki/Preemption_(computing)) scheduled onto some single-core systems). The reason is that such an operation is actually implemented as multiple machine instructions:

1. Fetch the value at the location *x*, say $x_{old}$, into a register;
2. add *a* to $x_{old}$ in the register;
3. store the new value of the register back into *x*.

When one process is doing x = x + a and another is doing x = x + b concurrently, there is a [race condition](https://en.wikipedia.org/wiki/Race_condition). They might both fetch  $x_{old}$ and operate on that, then both store their results with the effect that one overwrites the other and the stored value becomes either  $x_{old}$ + *a* or $x_{old}$ + *b*, not  $x_{old}$ + *a* + *b* as might be expected.

In [uniprocessor](https://en.wikipedia.org/wiki/Uniprocessor) systems with no [kernel preemption](https://en.wikipedia.org/wiki/Kernel_preemption) supported, it is sufficient to disable [interrupts](https://en.wikipedia.org/wiki/Interrupt) before accessing a [critical section](https://en.wikipedia.org/wiki/Critical_section). However, in multiprocessor systems (even with interrupts disabled) two or more processors could be attempting to access the same memory at the same time. The fetch-and-add instruction allows any processor to atomically increment a value in memory, preventing such multiple processor collisions.

[Maurice Herlihy](https://en.wikipedia.org/wiki/Maurice_Herlihy) (1991) proved that fetch-and-add has a finite [consensus](https://en.wikipedia.org/wiki/Consensus_(computer_science)) number, in contrast to the [compare-and-swap](https://en.wikipedia.org/wiki/Compare-and-swap) operation. The fetch-and-add operation can solve the wait-free consensus problem for no more than two concurrent processes.[[1\]](https://en.wikipedia.org/wiki/Fetch-and-add#cite_note-1)



### Implementation

The fetch-and-add instruction behaves like the following function. Crucially, the entire function is executed [atomically](https://en.wikipedia.org/wiki/Atomic_(computer_science)): no process can interrupt the function mid-execution and hence see a state that only exists during the execution of the function. This code only serves to help explain the behaviour of fetch-and-add; atomicity requires explicit hardware support and hence can not be implemented as a simple high level function.

```pseudocode
<< atomic >>
function FetchAndAdd(address location, int inc) {
    int value := *location
    *location := value + inc
    return value
}
```

To implement a mutual exclusion lock, we define the operation `FetchAndIncrement`, which is equivalent to `FetchAndAdd` with inc=1. With this operation, a mutual exclusion lock can be implemented using the [ticket lock](https://en.wikipedia.org/wiki/Ticket_lock) algorithm as:

```c
 record locktype {
    int ticketnumber
    int turn
 }
 procedure LockInit( locktype* lock ) {
    lock.ticketnumber := 0
    lock.turn := 0
 }
 procedure Lock( locktype* lock ) {
    int myturn := FetchAndIncrement( &lock.ticketnumber ) //must be atomic, since many threads might ask for a lock at the same time
    while lock.turn ≠ myturn 
        skip // spin until lock is acquired
 }
 procedure UnLock( locktype* lock ) {
    FetchAndIncrement( &lock.turn ) //this need not be atomic, since only the possessor of the lock will execute this
 }
```

These routines provide a mutual-exclusion lock when following conditions are met:

- Locktype data structure is initialized with function LockInit before use
- Number of tasks waiting for the lock does not exceed INT_MAX at any time
- Integer datatype used in lock values can 'wrap around' when continuously incremented



### Hardware and software support

An atomic fetch_add function appears in the [C++11](https://en.wikipedia.org/wiki/C%2B%2B11) standard.[[2\]](https://en.wikipedia.org/wiki/Fetch-and-add#cite_note-2) It is available as a proprietary extension to [C](https://en.wikipedia.org/wiki/C_(programming_language)) in the [Itanium](https://en.wikipedia.org/wiki/Itanium) [ABI](https://en.wikipedia.org/wiki/Application_binary_interface) specification,[[3\]](https://en.wikipedia.org/wiki/Fetch-and-add#cite_note-3) and (with the same syntax) in [GCC](https://en.wikipedia.org/wiki/GNU_Compiler_Collection).[[4\]](https://en.wikipedia.org/wiki/Fetch-and-add#cite_note-4)

#### x86 implementation

In the x86 architecture, the instruction `ADD` with the destination operand specifying a memory location is a fetch-and-add instruction that has been there since the [8086](https://en.wikipedia.org/wiki/8086) (it just wasn't called that then), and with the LOCK prefix, is atomic across multiple processors. However, it could not return the original value of the memory location (though it returned some flags) until the [486](https://en.wikipedia.org/wiki/80486) introduced the XADD instruction.

The following is a [C](https://en.wikipedia.org/wiki/C_(programming_language)) implementation for the [GCC](https://en.wikipedia.org/wiki/GNU_Compiler_Collection) compiler, for both 32 and 64 bit x86 Intel platforms, based on extended asm syntax:

```c
  static inline int fetch_and_add(int* variable, int value)
  {
      __asm__ volatile("lock; xaddl %0, %1"
        : "+r" (value), "+m" (*variable) // input+output
        : // No input-only
        : "memory"
      );
      return value;
  }
```

#### History

fetch-and-add was introduced by the [Ultracomputer](https://en.wikipedia.org/wiki/Ultracomputer) project, which also produced a multiprocessor supporting fetch-and-add and containing custom vlsi switches that were able to combine concurrent memory references (including fetch-and-adds) to prevent them from serializing at the memory module containing the destination operand.

### See also

- [Test-and-set](https://en.wikipedia.org/wiki/Test-and-set)
- [Test and Test-and-set](https://en.wikipedia.org/wiki/Test_and_Test-and-set)
- [Compare-and-swap](https://en.wikipedia.org/wiki/Compare-and-swap)
- [Load-Link/Store-Conditional](https://en.wikipedia.org/wiki/Load-Link/Store-Conditional)