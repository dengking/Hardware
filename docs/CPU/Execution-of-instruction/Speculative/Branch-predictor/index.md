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

## stackoverflow [Why is processing a sorted array faster than processing an unsorted array?](https://stackoverflow.com/questions/11227809/why-is-processing-a-sorted-array-faster-than-processing-an-unsorted-array)

Here is a piece of C++ code that shows some very peculiar behavior. For some strange reason, sorting the data miraculously makes the code almost six times faster:

```cpp
#include <algorithm>
#include <ctime>
#include <iostream>

int main()
{
	// Generate data
	const unsigned arraySize = 32768;
	int data[arraySize];

	for (unsigned c = 0; c < arraySize; ++c)
		data[c] = std::rand() % 256;

	// !!! With this, the next loop runs faster.
	std::sort(data, data + arraySize);

	// Test
	clock_t start = clock();
	long long sum = 0;

	for (unsigned i = 0; i < 100000; ++i)
	{
		// Primary loop
		for (unsigned c = 0; c < arraySize; ++c)
		{
			if (data[c] >= 128)
				sum += data[c];
		}
	}

	double elapsedTime = static_cast<double>(clock() - start) / CLOCKS_PER_SEC;

	std::cout << elapsedTime << std::endl;
	std::cout << "sum = " << sum << std::endl;
}
// g++ --std=c++11 test.cpp

```

- Without `std::sort(data, data + arraySize);`, the code runs in 11.54 seconds.
- With the sorted data, the code runs in 1.93 seconds.

------

Initially, I thought this might be just a language or compiler anomaly, so I tried Java:

```java
import java.util.Arrays;
import java.util.Random;

public class Main
{
    public static void main(String[] args)
    {
        // Generate data
        int arraySize = 32768;
        int data[] = new int[arraySize];

        Random rnd = new Random(0);
        for (int c = 0; c < arraySize; ++c)
            data[c] = rnd.nextInt() % 256;

        // !!! With this, the next loop runs faster
        Arrays.sort(data);

        // Test
        long start = System.nanoTime();
        long sum = 0;

        for (int i = 0; i < 100000; ++i)
        {
            // Primary loop
            for (int c = 0; c < arraySize; ++c)
            {
                if (data[c] >= 128)
                    sum += data[c];
            }
        }

        System.out.println((System.nanoTime() - start) / 1000000000.0);
        System.out.println("sum = " + sum);
    }
}
```

With a similar but less extreme result.

------

My first thought was that sorting brings the data into the [cache](https://en.wikipedia.org/wiki/CPU_cache), but then I thought how silly that was because the array was just generated.

- What is going on?
- Why is processing a sorted array faster than processing an unsorted array?

The code is summing up some independent terms, so the order should not matter.



### [A](https://stackoverflow.com/a/11227902)

**You are a victim of [branch prediction](https://en.wikipedia.org/wiki/Branch_predictor) fail.**

------

#### What is Branch Prediction?

> NOTE: 首先以train通过railroad junction为例来进行说明

Consider a railroad junction(交叉点):

 [Image](https://commons.wikimedia.org/wiki/File:Entroncamento_do_Transpraia.JPG) 

by Mecanismo, via Wikimedia Commons. Used under the [CC-By-SA 3.0](https://creativecommons.org/licenses/by-sa/3.0/deed.en) license.

Now for the sake of argument, suppose this is back in the 1800s - before long distance or radio communication.

You are the operator of a junction(连接) and you hear a train coming. You have no idea which way it is supposed to go. You stop the train to ask the driver which direction they want. And then you set the switch appropriately.

*Trains are heavy and have a lot of inertia. So they take forever to start up and slow down.*

Is there a better way? You guess which direction the train will go!

- If you guessed right, it continues on.
- If you guessed wrong, the captain(队长) will stop, back up, and yell at you to flip the switch. Then it can restart down the other path.

**If you guess right every time**, the train will never have to stop.
**If you guess wrong too often**, the train will spend a lot of time stopping, backing up, and restarting.

------

**Consider an if-statement:** At the processor level, it is a **branch instruction**:

![Screenshot of compiled code containing an if statement](https://i.stack.imgur.com/pyfwC.png)

You are a processor and you see a branch. You have no idea which way it will go. What do you do? You halt execution and wait until the previous instructions are complete. Then you continue down the correct path.

*Modern processors are complicated and have long pipelines. So they take forever to "warm up" and "slow down".*

Is there a better way? You guess which direction the branch will go!

- If you guessed right, you continue executing.
- If you guessed wrong, you need to flush the pipeline and roll back to the branch. Then you can restart down the other path.

**If you guess right every time**, the execution will never have to stop.
**If you guess wrong too often**, you spend a lot of time stalling, rolling back, and restarting.

------

This is **branch prediction**. I admit it's not the best analogy since the train could just signal the direction with a flag. But in computers, the processor doesn't know which direction a branch will go until the last moment.

So how would you strategically guess to minimize the number of times that the train must back up and go down the other path? You look at the past history! If the train goes left 99% of the time, then you guess left. If it alternates, then you alternate your guesses. If it goes one way every three times, you guess the same...

***In other words, you try to identify a pattern and follow it.*** This is more or less how branch predictors work.

Most applications have well-behaved branches. So modern branch predictors will typically achieve >90% hit rates. But when faced with unpredictable branches with no recognizable patterns, **branch predictors** are virtually useless.

Further reading: ["Branch predictor" article on Wikipedia](https://en.wikipedia.org/wiki/Branch_predictor).

------

#### As hinted from above, the culprit(元凶) is this if-statement:

```c++
if (data[c] >= 128)
    sum += data[c];
```

Notice that the data is evenly distributed between 0 and 255. When the data is sorted, roughly the first half of the iterations will not enter the if-statement. After that, they will all enter the if-statement.

This is very friendly to the **branch predictor** since the branch consecutively goes the same direction many times. Even a simple saturating counter will correctly predict the branch except for the few iterations after it switches direction.

**Quick visualization:**

```
T = branch taken
N = branch not taken

data[] = 0, 1, 2, 3, 4, ... 126, 127, 128, 129, 130, ... 250, 251, 252, ...
branch = N  N  N  N  N  ...   N    N    T    T    T  ...   T    T    T  ...

       = NNNNNNNNNNNN ... NNNNNNNTTTTTTTTT ... TTTTTTTTTT  (easy to predict)
```

However, when the data is completely random, the branch predictor is rendered useless, because it can't predict random data. Thus there will probably be around 50% misprediction (no better than random guessing).

```
data[] = 226, 185, 125, 158, 198, 144, 217, 79, 202, 118,  14, 150, 177, 182, 133, ...
branch =   T,   T,   N,   T,   T,   T,   T,  N,   T,   N,   N,   T,   T,   T,   N  ...

       = TTNTTTTNTNNTTTN ...   (completely random - hard to predict)
```

------

**So what can be done?**

If the compiler isn't able to optimize the branch into a conditional move, you can try some hacks if you are willing to sacrifice readability for performance.

Replace:

```
if (data[c] >= 128)
    sum += data[c];
```

with:

```
int t = (data[c] - 128) >> 31;
sum += ~t & data[c];
```

This eliminates the branch and replaces it with some bitwise operations.

(Note that this hack is not strictly equivalent to the original if-statement. But in this case, it's valid for all the input values of `data[]`.)

**Benchmarks: Core i7 920 @ 3.5 GHz**

C++ - Visual Studio 2010 - x64 Release

```
//  Branch - Random
seconds = 11.777

//  Branch - Sorted
seconds = 2.352

//  Branchless - Random
seconds = 2.564

//  Branchless - Sorted
seconds = 2.587
```

Java - NetBeans 7.1.1 JDK 7 - x64

```
//  Branch - Random
seconds = 10.93293813

//  Branch - Sorted
seconds = 5.643797077

//  Branchless - Random
seconds = 3.113581453

//  Branchless - Sorted
seconds = 3.186068823
```

Observations:

- **With the Branch:** There is a huge difference between the sorted and unsorted data.
- **With the Hack:** There is no difference between sorted and unsorted data.
- In the C++ case, the hack is actually a tad slower than with the branch when the data is sorted.

A general rule of thumb is to avoid data-dependent branching in critical loops (such as in this example).

------

**Update:**

- GCC 4.6.1 with `-O3` or `-ftree-vectorize` on x64 is able to generate a conditional move. So there is no difference between the sorted and unsorted data - both are fast.

    (Or somewhat fast: for the already-sorted case, `cmov` can be slower especially if GCC puts it on the critical path instead of just `add`, especially on Intel before Broadwell where `cmov` has 2 cycle latency: [gcc optimization flag -O3 makes code slower than -O2](https://stackoverflow.com/questions/28875325/gcc-optimization-flag-o3-makes-code-slower-than-o2))

- VC++ 2010 is unable to generate conditional moves for this branch even under `/Ox`.

- [Intel C++ Compiler](https://en.wikipedia.org/wiki/Intel_C%2B%2B_Compiler) (ICC) 11 does something miraculous. It [interchanges the two loops](https://en.wikipedia.org/wiki/Loop_interchange), thereby hoisting the unpredictable branch to the outer loop. So not only is it immune to the mispredictions, it is also twice as fast as whatever VC++ and GCC can generate! In other words, ICC took advantage of the test-loop to defeat the benchmark...

- If you give the Intel compiler the branchless code, it just out-right vectorizes it... and is just as fast as with the branch (with the loop interchange).

This goes to show that even mature modern compilers can vary wildly in their ability to optimize code...