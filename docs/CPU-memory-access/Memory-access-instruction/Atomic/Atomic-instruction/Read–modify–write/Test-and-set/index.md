# Test-and-set



## wikipedia [Test-and-set](http://en.wikipedia.org/wiki/Test-and-set)

In computer science, the test-and-set instruction is an instruction used to write 1 (set) to a memory location and return its old value as a single atomic (i.e., non-interruptible) operation. If multiple processes may access the same memory location, and if a process is currently performing a test-and-set, no other process may begin another test-and-set until the first process's test-and-set is finished. A CPU may use a test-and-set instruction offered by another electronic component, such as dual-port RAM; a CPU itself may also offer a test-and-set instruction.