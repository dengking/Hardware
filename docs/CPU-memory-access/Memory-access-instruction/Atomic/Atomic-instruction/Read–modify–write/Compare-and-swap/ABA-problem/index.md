# ABA problem

在阅读 preshing [An Introduction to Lock-Free Programming](https://preshing.com/20120612/an-introduction-to-lock-free-programming/) 时，其中介绍了ABA problem。

## 总结

1、ABA也是由于race而引发的

2、ABA和time of check to time of use是非常类似的

## Wikipedia [ABA problem](https://infogalactic.com/info/ABA_problem)

In [multithreaded](https://en.wikipedia.org/wiki/Thread_(computer_science)) [computing](https://en.wikipedia.org/wiki/Computer_science), the **ABA problem** occurs during synchronization, when a location is read twice, has the same value for both reads, and "value is the same" is used to indicate "nothing has changed". However, another thread can execute between the two reads and change the value, do other work, then change the value back, thus fooling the first thread into thinking "nothing has changed" even though the second thread did work that violates that assumption.

In [multithreaded](https://en.wikipedia.org/wiki/Thread_(computer_science)) [computing](https://en.wikipedia.org/wiki/Computer_science), the **ABA problem** occurs during synchronization, when a location is read twice, has the same value for both reads, and "value is the same" is used to indicate "nothing has changed". However, another thread can execute between the two reads and change the value, do other work, then change the value back, thus fooling the first thread into thinking "nothing has changed" even though the second thread did work that violates that assumption.

The ABA problem occurs when multiple [threads](https://en.wikipedia.org/wiki/Thread_(computer_science)) (or [processes](https://en.wikipedia.org/wiki/Process_(computing))) accessing shared data interleave. Below is the sequence of events that will result in the ABA problem:

- Process ${P_{1}}$ reads value A from shared memory,
- ${P_{1}}$ is [preempted](https://en.wikipedia.org/wiki/Preemption_(computing)), allowing process ${P_{2}}$ to run,
- ${P_{2}}$modifies the shared memory value A to value B and back to A before preemption,
- ${P_{1}}$ begins execution again, sees that the shared memory value has not changed and continues.

Although ${P_{1}}$ can continue executing, it is possible that the behavior will not be correct due to the "hidden" modification in shared memory.

A common case of the ABA problem is encountered when implementing a [lock-free](https://en.wikipedia.org/wiki/Lock-free) data structure. If an item is removed from the list, deleted, and then a new item is allocated and added to the list, it is common for the allocated object to be at the same location as the deleted object due to [MRU](https://en.wikipedia.org/wiki/Cache_replacement_policies#Most_recently_used_(MRU)) memory allocation. A pointer to the new item is thus often equal to a pointer to the old item, causing an ABA problem.

> NOTE: [MRU](https://en.wikipedia.org/wiki/Cache_replacement_policies#Most_recently_used_(MRU))所表示的是Most recently used

### Examples

Consider a software example of ABA using a [lock-free](https://en.wikipedia.org/wiki/Lock-free) [stack](https://en.wikipedia.org/wiki/Stack_(data_structure)):

```C++
#include <iostream>
#include <utility>
#include <vector>
#include <atomic>

using namespace std;

class Obj
{
public:
	Obj *next { nullptr };
};
/* Naive lock-free stack which suffers from ABA problem.*/
class Stack
{
	std::atomic<Obj*> top_ptr;
	//
	// Pops the top object and returns a pointer to it.
	//
	Obj* Pop()
	{
		while (1)
		{
			Obj *ret_ptr = top_ptr;
			if (!ret_ptr)
				return nullptr;
			// For simplicity, suppose that we can ensure that this dereference is safe
			// (i.e., that no other thread has popped the stack in the meantime).
			Obj *next_ptr = ret_ptr->next;
			// If the top node is still ret, then assume no one has changed the stack.
			// (That statement is not always true because of the ABA problem)
			// Atomically replace top with next.
			if (top_ptr.compare_exchange_weak(ret_ptr, next_ptr))
			{
				return ret_ptr;
			}
			// The stack has changed, start over.
		}
	}
	//
	// Pushes the object specified by obj_ptr to stack.
	//
	void Push(Obj *obj_ptr)
	{
		while (1)
		{
			Obj *next_ptr = top_ptr;
			obj_ptr->next = next_ptr;
			// If the top node is still next, then assume no one has changed the stack.
			// (That statement is not always true because of the ABA problem)
			// Atomically replace top with obj.
			if (top_ptr.compare_exchange_weak(next_ptr, obj_ptr))
			{
				return;
			}
			// The stack has changed, start over.
		}
	}
};

int main()
{

}
// g++ -std=c++11  -Wall -pedantic -pthread main.cpp && ./a.out


```

### Workarounds

#### Tagged state reference

#### Intermediate nodes

#### Deferred reclamation



## Workarounds总结

### Herb Sutter [Lock-Free Programming or, How to Juggle Razor Blades](http://www.alfasoft.com/files/herb/40-LockFree.pdf) 

在 Herb Sutter [Lock-Free Programming or, How to Juggle Razor Blades](http://www.alfasoft.com/files/herb/40-LockFree.pdf) 中，给出了总结，这篇文章收录于工程programming-language的`Lock-Free-Programming-or-How-to-Juggle-Razor-Blades`章节。

ABA Solutions (sketch)

We need to solve the ABA issue: Two nodes with the same address, but different identities (existing at different times).

Option 1: Use lazy garbage collection.

Solves the problem. Memory can’t be reused while pointers to it exist.

But: Not an option (yet) in portable C++ code, and destruction of nodes becomes nondeterministic.

Option 2: Use reference counting (garbage collection).

Solves the problem in cases without cycles. Again, avoids memory reuse.

Option 3: Make each pointer unique by appending a serial number, and increment the serial number each time it’s set.

This way we can always distinguish between A and A’.

But: Requires an atomic compare and swap on a value that’s larger than the size of a pointer. Not available on all hardware & bit nesses.

Option 4: Use hazard pointers.

Maged Michael and Andrei Alexandrescu have covered this in detail. But: It’s very intricate(复杂的). Tread(踩踏) with caution.

### ticki [Fearless concurrency with hazard pointers](http://ticki.github.io/blog/fearless-concurrency-with-hazard-pointers/)