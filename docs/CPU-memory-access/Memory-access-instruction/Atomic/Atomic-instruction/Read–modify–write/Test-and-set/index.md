# Test-and-set



## wikipedia [Test-and-set](http://en.wikipedia.org/wiki/Test-and-set)

In computer science, the test-and-set instruction is an instruction used to write `1` (set) to a memory location and return its old value as a single atomic (i.e., non-interruptible) operation. If multiple processes may access the same memory location, and if a process is currently performing a test-and-set, no other process may begin another test-and-set until the first process's test-and-set is finished. A CPU may use a test-and-set instruction offered by another electronic component, such as dual-port RAM; a CPU itself may also offer a test-and-set instruction.

> NOTE: 
>
> 一、shared data会不断地被写入1，会导致cache coherence flood、high interconnect contention
>
> 二、
>
> 如果 `lock` 的值是1，那么`test_and_set(lock) == 1`就会是true，则     `while (test_and_set(lock) == 1);`  就一直无法退出；
>
> 如果`lock` 的值是0，那么`test_and_set(lock) == 1`就会是false，则     `while (test_and_set(lock) == 1);`  就立即退出；
>
> 第一个获得lock的人，会将它的值设置为1，因此就能够避免所有其他的人也获得锁，在它使用完成后，则将锁释放，其他的人就能够获得锁了。

```C++
function Lock(boolean *lock) { 
    while (test_and_set(lock) == 1); 
}
```

The calling process obtains the lock if the old value was 0, otherwise the while-loop spins waiting to acquire the lock. This is called a [spinlock](https://en.wikipedia.org/wiki/Spinlock). "[Test and Test-and-set](https://en.wikipedia.org/wiki/Test_and_test-and-set)" is another example.

### Mutual exclusion using test-and-set

#### Pseudo-C implementation

```C++
volatile int lock = 0;

void critical() {
    while (test_and_set(&lock) == 1);
    critical section  // only one process can be in this section at a time
    lock = 0  // release lock when finished with the critical section
}
```

### Performance evaluation of test-and-set locks

The four major evaluation metrics for locks in general are:

1、uncontended lock-acquisition latency

2、bus traffic

3、fairness

4、storage

> NOTE: 
>
> 上述总结的是非常好的

Test-and-set scores low on two of them, namely, **high bus traffic** and **unfairness**.



## Example

[std::atomic_flag](https://en.cppreference.com/w/cpp/atomic/atomic_flag)

