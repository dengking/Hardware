
# [How to tell if a Linux system is big endian or little endian?](https://serverfault.com/questions/163487/how-to-tell-if-a-linux-system-is-big-endian-or-little-endian)





# [Is there a system command, in Linux, that reports the endianness?](https://unix.stackexchange.com/questions/88934/is-there-a-system-command-in-linux-that-reports-the-endianness)





# [Little and Big Endian Mystery](https://www.geeksforgeeks.org/little-and-big-endian-mystery/)

**What are these?**
Little and big endian are two ways of storing multibyte data-types ( `int`, `float`, etc). In little endian machines, last byte of binary representation of the multibyte data-type is stored first. On the other hand, in big endian machines, first byte of binary representation of the multibyte data-type is stored first.

***SUMMARY*** : 参见[Endianness](https://en.wikipedia.org/wiki/Endianness)，其中的解释比这里更好：A **little-endian** ordering places the least significant byte first and the most significant byte last, while a **big-endian** ordering does the opposite. 也就是将least significant byte置于低地址，将most significant byte置于高地址；

Suppose integer is stored as 4 bytes (For those who are using DOS based compilers such as C++ 3.0 , integer is 2 bytes) then a variable x with value `0x01234567` will be stored as following.

Memory representation of integer `0x01234567` inside Big and little endian machines

**How to see memory representation of multibyte data types on your machine?**

Here is a sample C code that shows the byte representation of int, float and pointer.

```c
#include <stdio.h> 
  
/* function to show bytes in memory, from location start to start+n*/
void show_mem_rep(char *start, int n)  
{ 
    int i; 
    for (i = 0; i < n; i++) 
         printf(" %.2x", start[i]); 
    printf("\n"); 
} 
  
/*Main function to call above function for 0x01234567*/
int main() 
{ 
   int i = 0x01234567; // 67是least significant byte,01是most significant byte
   show_mem_rep((char *)&i, sizeof(i)); 
   return 0; 
} 
```

When above program is run on little endian machine, gives “67 45 23 01” as output , while if it is run on big endian machine, gives “01 23 45 67” as output.

**Is there a quick way to determine endianness of your machine?**

There are n no. of ways for determining endianness of your machine. Here is one quick way of doing the same.

```c
#include <stdio.h> 
int main()  
{ 
   unsigned int i = 1; 
   char *c = (char*)&i; 
   if (*c)     
       printf("Little endian"); 
   else
       printf("Big endian"); 
   getchar(); 
   return 0; 
} 
```

```c++
#include <bits/stdc++.h> 
using namespace std; 
int main()  
{  
    unsigned int i = 1;  
    char *c = (char*)&i;  
    if (*c)  
        cout<<"Little endian";  
    else
        cout<<"Big endian";  
    return 0;  
}  
  
// This code is contributed by rathbhupendra 
```

**Output:**

In the above program, a character pointer c is pointing to an integer i. Since size of character is 1 byte when the character pointer is de-referenced it will contain only first byte of integer. If machine is little endian then `*c` will be 1 (because last byte is stored first) and if machine is big endian then `*c` will be 0.





**Does endianness matter for programmers?**

Most of the times compiler takes care of endianness, however, endianness becomes an issue in following cases.

It matters in **network programming**: Suppose you write integers to file on a **little endian machine** and you transfer this file to a **big endian machine**. Unless there is **little endian** to **big endian** transformation, **big endian machine** will read the file in reverse order. You can find such a practical example here.

Standard byte order for networks is **big endian**, also known as **network byte order**. Before transferring data on network, data is first converted to **network byte order** (**big endian**).

Sometimes it matters when you are using **type casting**, below program is an example.

```c
#include <stdio.h> 
int main() 
{ 
    unsigned char arr[2] = {0x01, 0x00}; 
    unsigned short int x = *(unsigned short int *) arr; 
    printf("%d", x); 
    getchar(); 
    return 0; 
} 
```



In the above program, a char array is typecasted to an unsigned short integer type. When I run above program on little endian machine, I get `1` as output, while if I run it on a big endian machine I get `256`. To make programs endianness independent, above programming style should be avoided.



***SUMMARY*** : `unsigned short int x = *(unsigned short int *) arr; `语句的效果是使`x`的内存image为`0X0100`，对于这篇内存的解释是机器相关的，在big endian machine上，它是`256`，在little endian machine上，它是`1`。



***SUMMARY*** : [Writing endian-independent code in C](https://developer.ibm.com/articles/au-endianc/)





# [C program to check little vs. big endian [duplicate\]](https://stackoverflow.com/questions/12791864/c-program-to-check-little-vs-big-endian)



# [C Macro definition to determine big endian or little endian machine?](https://stackoverflow.com/questions/2100331/c-macro-definition-to-determine-big-endian-or-little-endian-machine)

