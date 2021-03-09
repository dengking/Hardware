# How to get Word length 



## stackoverflow [How to determine processor word length in C?](https://stackoverflow.com/questions/29519068/how-to-determine-processor-word-length-in-c)



### [A](https://stackoverflow.com/a/29519151)

As [@holgac mentioned](https://stackoverflow.com/questions/29519068/how-to-determine-processor-word-length-in-c/29519151?noredirect=1#comment47194315_29519151), the `long` datatype is always the same size as the machine's native word size:

> "A word is the amount of data that a machine can process at one time." "The size of a processor’s general-purpose registers (GPRs) is equal to its word size." "Additionally, the size of the C type `long` is equal to the word size, whereas the size of the int type is sometimes less than that of the word size"
>
> -- Linux Kernel Development, Ch 17 (3rd edition, pg 381)

As [indicated by Thomas Matthews](https://stackoverflow.com/questions/29519068/how-to-determine-processor-word-length-in-c/29519151#comment47194967_29519151) however, this may not apply to machines with small word lengths.

To determine the size of `long` on your compiler, just use `sizeof(long)`:

```C++
#include"stdio.h"
#include <limits.h>

int main(void)
{
	printf("long is %d bits on this system\n", (int) sizeof(long) * CHAR_BIT);
	return 0;
}

```

See also:

1、[What is CHAR_BIT?](https://stackoverflow.com/q/3200954/119527)



## C program that get the architecture（32 or 64 bit ) 

```C++
void initServerConfig(void) {
    server.arch_bits = (sizeof(long) == 8) ? 64 : 32;
}    
```

这段代码中使用的方法是从redis的`server.c`的`initServerConfig`函数中截取过来的，下面是测试程序: 

```c
#include<iostream>
int main()
{
	int arch_bits = (sizeof(long) == 8) ? 64 : 32;
	std::cout << arch_bits << std::endl;
}

```



