https://en.wikipedia.org/wiki/Call_stack

> 观书有感：其实我在学习栈帧的时候，并不一定是要完全局限于栈帧的知识，更加重要的是在计算机科学中，栈这样的一个简单的结构，它有着怎样的特性，使它能够作为CPU函数调用的实现结构？
>
> 计算机科学中许多事务的一个特性是简单，连续。
>
> 现在想起来，之前在学习递归函数的时候，其实已经学习了栈的知识。
>
> 

## Description

Since the call stack is organized as a [stack](https://en.wikipedia.org/wiki/Stack_(abstract_data_type)), the caller pushes the return address<!--return address是什么呢？显然，什么地方调用了这个函数，那么在这个函数执行完成后，就需要返回到这个地方来，显然，在CPU中，这个地方就是使用地址来进行确定的。CPU就需要将控制传输到这个地方--> onto the stack, and the called subroutine, when it finishes, [pulls or pops](https://en.wikipedia.org/wiki/Pop_(computer_programming)) the return address off the call stack and transfers control to that address.<!--是先push the return address还是先执行函数再push the return address？显然是先push the return address。这是一个可重复，连续的过程 -->If a called subroutine calls on yet another subroutine<!--被调用函数又调用了另外一个函数-->, it will push another return address onto the call stack, and so on, with the information stacking up and unstacking as the program dictates. If the pushing consumes all of the space allocated for the call stack, an error called a [stack overflow](https://en.wikipedia.org/wiki/Stack_overflow) occurs, generally causing the program to [crash](https://en.wikipedia.org/wiki/Crash_(computing)). Adding a subroutine's entry to the call stack is sometimes called "winding"; conversely, removing entries is "unwinding".<!--这个地方所说的Adding a subroutine's entry和前面所描述的return address是同一个东西吗？-->

There is usually exactly one call stack associated with a running program (or more accurately, with each [task](https://en.wikipedia.org/wiki/Task_(computers)) or [thread](https://en.wikipedia.org/wiki/Thread_(computer_science)) of a [process](https://en.wikipedia.org/wiki/Process_(computing))), although additional stacks may be created for [signal](https://en.wikipedia.org/wiki/Signal_(computing)) handling or [cooperative multitasking](https://en.wikipedia.org/wiki/Cooperative_multitasking) (as with [setcontext](https://en.wikipedia.org/wiki/Setcontext)). Since there is only one in this important context, it can be referred to as *the* stack (implicitly, "of the task"); however, in the [Forth programming language](https://en.wikipedia.org/wiki/Forth_programming_language) the *data stack* or *parameter stack* is accessed more explicitly than the call stack and is commonly referred to as *the* stack (see below).

In [high-level programming languages](https://en.wikipedia.org/wiki/High-level_programming_language), the specifics of the call stack are usually hidden from the programmer. They are given access only to a set of functions, and not the memory on the stack itself. This is an example of [abstraction](https://en.wikipedia.org/wiki/Abstraction_(computer_science)). Most [assembly languages](https://en.wikipedia.org/wiki/Assembly_language), on the other hand, require programmers to be involved with manipulating the stack. The actual details of the stack in a [programming language](https://en.wikipedia.org/wiki/Programming_language) depend upon the [compiler](https://en.wikipedia.org/wiki/Compiler), [operating system](https://en.wikipedia.org/wiki/Operating_system), and the available [instruction set](https://en.wikipedia.org/wiki/Instruction_set).

## Functions of the call stack

As noted above, the primary purpose of a call stack is to *store the return addresses*. When a subroutine is called<!--被调用-->, the location (address) of the instruction at which the calling routine<!--参见在description章节中的return address--> can later resume needs to be saved somewhere. Using a stack to save the return address has important advantages over alternative [calling conventions](https://en.wikipedia.org/wiki/Calling_convention). One is that each task<!--注意，这里使用的是task，而不是process，thread，显然使用task是更加准确的，因为它表示的是一个比process，thread更加抽象的执行单位--> can have its own stack, and thus the subroutine can be [thread-safe](https://en.wikipedia.org/wiki/Thread_safety), that is, can be active simultaneously for different tasks doing different things. Another benefit is that by providing [reentrancy](https://en.wikipedia.org/wiki/Reentrancy_(computing)), [recursion](https://en.wikipedia.org/wiki/Recursion_(computer_science)) is automatically supported. When a function calls itself recursively, a return address needs to be stored for each activation of the function so that it can later be used to return from the function activation. Stack structures provide this capability automatically.

Depending on the language, operating-system, and machine environment, a call stack may serve additional purposes, including for example:

- Local data storage

See also: [Stack-based memory allocation](https://en.wikipedia.org/wiki/Stack-based_memory_allocation)

- Parameter passing<!--传参-->

  Subroutines often require that values for [parameters](https://en.wikipedia.org/wiki/Parameter_(computer_science)) be supplied to them by the code which calls them<!--函数传参-->, and it is not uncommon that space for these parameters may be laid out in the call stack<!--其实使用栈帧来进行函数传参是比较少的-->. Generally if there are only a few small parameters, [processor registers](https://en.wikipedia.org/wiki/Processor_register) will be used to pass the values<!--通常是使用processor register来进行函数传参-->, but if there are more parameters than can be handled this way, memory space will be needed<!--当函数的参数的个数超过processor register时候，就需要使用memory space了-->. The call stack works well as a place for these parameters, especially since each call to a subroutine, which will have differing values for parameters, will be given separate space on the call stack for those values.

- Evaluation stack

  Operands for arithmetic or logical operations are most often placed into registers and operated on there. However, in some situations the operands may be stacked up to an arbitrary depth, which means something more than registers must be used (this is the case of [register spilling](https://en.wikipedia.org/wiki/Register_allocation#Spilling)). The stack of such operands, rather like that in an [RPN calculator](https://en.wikipedia.org/wiki/RPN_calculator), is called an evaluation stack, and may occupy space in the call stack.

- [Pointer](https://en.wikipedia.org/wiki/Pointer_(computer_programming)) to current instance

  Some [object-oriented languages](https://en.wikipedia.org/wiki/Object-oriented_language) (e.g., [C++](https://en.wikipedia.org/wiki/C%2B%2B)), store the [*this* pointer](https://en.wikipedia.org/wiki/This_(computer_science)) along with function arguments in the call stack when invoking methods. The *this* pointer points to the [object](https://en.wikipedia.org/wiki/Object_(computer_science)) [instance](https://en.wikipedia.org/wiki/Instance_(computer_science)) associated with the method to be invoked.

- Enclosing subroutine context<!--闭包特性-->

  Some programming languages (e.g., [Pascal](https://en.wikipedia.org/wiki/Pascal_(programming_language)) and [Ada](https://en.wikipedia.org/wiki/Ada_(programming_language))) support declaration of [nested subroutines](https://en.wikipedia.org/wiki/Nested_functions), which are allowed to access the context of their enclosing routines, i.e., the parameters and local variables within the scope of the outer routines. Such static nesting can repeat - a function declared within a function declared within a function... The implementation must provide a means by which a called function at any given static nesting level can reference the enclosing frame at each enclosing nesting level. Commonly this reference is implemented by a pointer to the frame of the most recently activated instance of the enclosing function, called a "downstack link" or "static link", to distinguish it from the "dynamic link" that refers to the immediate caller (which need not be the static parent function).


- Other return state

  Beside the return address, in some environments there may be other machine or software states that need to be restored when a subroutine returns. This might include things like privilege level, exception-handling information, arithmetic modes, and so on. If needed, this may be stored in the call stack just as the return address is.<!--和return address一样-->

The typical call stack is used for the return address, locals, and parameters (known as a *call frame*). In some environments there may be more or fewer functions assigned to the call stack. In the [Forth programming language](https://en.wikipedia.org/wiki/Forth_(programming_language)), for example, ordinarily only the return address, counted loop parameters and indexes, and possibly local variables are stored on the call stack (which in that environment is named the *return stack*), although any data can be temporarily placed there using special return-stack handling code so long as the needs of calls and returns are respected; parameters are ordinarily stored on a separate *data stack* or *parameter stack*, typically called *the* stack in Forth terminology even though there is a call stack since it is usually accessed more explicitly. Some Forths also have a third stack for [floating-point](https://en.wikipedia.org/wiki/Floating-point)parameters.

> 观书有感：这几天，看这些底层的计算机知识，才发现原来在高级语言中，我平时接触到的许多的概念，其实都是基于底层的实现机制而提出的。

## Structure

A **call stack** is composed of **stack frames** (also called *activation records* or *activation frames*). These are [machine dependent](https://en.wikipedia.org/wiki/Machine_dependent) and [ABI](https://en.wikipedia.org/wiki/Application_binary_interface)-dependent data structures containing subroutine state information. This data is sometimes referred to as CFI (Call Frame Information).[[1\]](https://en.wikipedia.org/wiki/Call_stack#cite_note-1) Each stack frame corresponds to a call to a subroutine which has not yet terminated with a return. For example, if a subroutine named `DrawLine` is currently running, having been called by a subroutine `DrawSquare`, the top part of the call stack might be laid out like in the picture on the right.

![](https://upload.wikimedia.org/wikipedia/commons/thumb/d/d3/Call_stack_layout.svg/342px-Call_stack_layout.svg.png)

A diagram like this can be drawn in either direction as long as the placement of the top, and so direction of stack growth, is understood. Furthermore, independently of this, architectures differ as to whether call stacks grow towards higher addresses or towards lower addresses. The logic of the diagram is independent of the addressing choice.

The stack frame at the top of the stack is for the currently executing routine. The stack frame usually includes at least the following items (in push order):

- the arguments (parameter values) passed to the routine (if any);
- the return address back to the routine's caller (e.g. in the `DrawLine` stack frame, an address into `DrawSquare`'s code); and
- space for the local variables of the routine (if any).

### Stack and frame pointers

When stack frame sizes can differ, such as between different functions or between invocations of a particular function, popping a frame off the stack does not constitute a fixed decrement of the **stack pointer**<!--因为栈帧的大小不同，比如不同的函数，所以pop一个栈帧的时候，stack pointer并不一定减少一个固定的大小-->. At function return, the stack pointer is instead restored to the **frame pointer**<!--当函数返回的时候，stack pointer被设置为frame pointer的值-->, the value of the **stack pointer** just before the function was called<!--即函数被调用之前的stack pointer的值，这其实是一种恢复-->. Each **stack frame** contains a **stack pointer** to the top of the frame immediately below. The stack pointer is a mutable register shared between all invocations. A frame pointer of a given invocation of a function is a copy of the stack pointer as it was before the function was invoked<!--一个函数调用的frame pointer是该函数被调用之前的堆栈指针的副本-->.[[2\]](https://en.wikipedia.org/wiki/Call_stack#cite_note-2)

The locations of all other fields in the frame can be defined relative either to the top of the frame, as negative offsets of the stack pointer, or relative to the top of the frame below, as positive offsets of the frame pointer. The location of the frame pointer itself must inherently be defined as a negative offset of the stack pointer.<!--帧中所有其他字段的位置可以相对于帧的顶部相对于堆栈指针的负偏移或相对于下面帧的顶部而定义为帧指针的正偏移。 帧指针本身的位置必须固有地定义为堆栈指针的负偏移量。-->

> 观书有感：无论是栈还是堆，它们都是属于内存的范轴。

### Storing the address to the caller's frame

In most systems a **stack frame** has a field to contain the previous value of the frame pointer register, the value it had while the caller was executing<!--在大多数系统中，一个堆栈帧有一个字段来包含在该函数被调用之前帧指针寄存器的值，即调用者执行时的值-->. For example, the stack frame of `DrawLine` would have a memory location holding the frame pointer value that `DrawSquare` uses (not shown in the diagram above). The value is saved upon entry to the subroutine and restored upon return<!--在调用函数的时候，这个值被压入到栈中，在函数返回时，则进行恢复-->. Having such a field in a known location in the stack frame enables code to access each frame successively underneath<!----> the currently executing routine's frame<!--也就是这是实现前面提到的Enclosing subroutine context的实现基础-->, and also allows the routine to easily restore the frame pointer to the *caller's* frame, just before it returns.

### Lexically nested routines

Further information: [Nested function](https://en.wikipedia.org/wiki/Nested_function) and [Non-local variable](https://en.wikipedia.org/wiki/Non-local_variable)

Programming languages that support [nested subroutines](https://en.wikipedia.org/wiki/Nested_function) also have a field in the call frame that points to the stack frame of the *latest* activation of the procedure that most closely encapsulates the callee, i.e. the immediate *scope* of the callee. This is called an *access link* or *static link* (as it keeps track of static nesting during dynamic and recursive calls) and provides the routine (as well as any other routines it may invoke) access to the local data of its encapsulating routines at every nesting level. Some architectures, compilers, or optimization cases store one link for each enclosing level (not just the immediately enclosing), so that deeply nested routines that access shallow data do not have to traverse several links; this strategy is often called a "display".[[3\]](https://en.wikipedia.org/wiki/Call_stack#cite_note-3)

Access links can be optimized away when an inner function does not access any (non-constant) local data in the encapsulation, as is the case with pure functions communicating only via arguments and return values, for example. Some historical computers, such as the [Burroughs large systems](https://en.wikipedia.org/wiki/Burroughs_large_systems), had special "display registers" to support nested functions, while compilers for most modern machines (such as the ubiquitous x86) simply reserve a few words on the stack for the pointers, as needed.

### Overlap

For some purposes, the stack frame of a subroutine and that of its caller can be considered to overlap, the overlap consisting of the area where the parameters are passed from the caller to the callee. In some environments, the caller pushes each argument onto the stack, thus extending its stack frame, then invokes the callee. In other environments, the caller has a preallocated area at the top of its stack frame to hold the arguments it supplies to other subroutines it calls. This area is sometimes termed the *outgoing arguments area* or *callout area*. Under this approach, the size of the area is calculated by the compiler to be the largest needed by any called subroutine.

## Use[[edit](https://en.wikipedia.org/w/index.php?title=Call_stack&action=edit&section=8)]

### Call site processing[[edit](https://en.wikipedia.org/w/index.php?title=Call_stack&action=edit&section=9)]

Usually the call stack manipulation needed at the site of a call to a subroutine is minimal (which is good since there can be many call sites for each subroutine to be called). The values for the actual arguments are evaluated at the call site, since they are specific to the particular call, and either pushed onto the stack or placed into registers, as determined by the used [calling convention](https://en.wikipedia.org/wiki/Calling_convention). The actual call instruction, such as "branch and link", is then typically executed to transfer control to the code of the target subroutine.

### Subroutine entry processing[[edit](https://en.wikipedia.org/w/index.php?title=Call_stack&action=edit&section=10)]

In the called subroutine, the first code executed is usually termed the [subroutine prologue](https://en.wikipedia.org/wiki/Function_prologue), since it does the necessary housekeeping before the code for the statements of the routine is begun.

The prologue will commonly save the return address left in a register by the call instruction by pushing the value onto the call stack. Similarly, the current stack pointer and/or frame pointer values may be pushed. Alternatively, some instruction set architectures automatically provide comparable functionality as part of the action of the call instruction itself, and in such an environment the prologue does not need to do this.

If frame pointers are being used, the prologue will typically set the new value of the frame pointer register from the stack pointer. Space on the stack for local variables can then be allocated by incrementally changing the stack pointer.

The [Forth programming language](https://en.wikipedia.org/wiki/Forth_(programming_language)) allows explicit winding of the call stack (called there the "return stack").

### Return processing[[edit](https://en.wikipedia.org/w/index.php?title=Call_stack&action=edit&section=11)]

When a subroutine is ready to return, it executes an epilogue that undoes the steps of the prologue. This will typically restore saved register values (such as the frame pointer value) from the stack frame, pop the entire stack frame off the stack by changing the stack pointer value, and finally branch to the instruction at the return address. Under many calling conventions the items popped off the stack by the epilogue include the original argument values, in which case there usually are no further stack manipulations that need to be done by the caller. With some calling conventions, however, it is the caller's responsibility to remove the arguments from the stack after the return.

### Unwinding[[edit](https://en.wikipedia.org/w/index.php?title=Call_stack&action=edit&section=12)]

Returning from the called function will pop the top frame off of the stack, perhaps leaving a return value. The more general act of popping one or more frames off the stack to resume execution elsewhere in the program is called **stack unwinding** and must be performed when non-local control structures are used, such as those used for [exception handling](https://en.wikipedia.org/wiki/Exception_handling#Exception_handling_in_software). In this case, the stack frame of a function contains one or more entries specifying exception handlers. When an exception is thrown, the stack is unwound until a handler is found that is prepared to handle (catch) the type of the thrown exception.

Some languages have other control structures that require general unwinding. [Pascal](https://en.wikipedia.org/wiki/Pascal_programming_language) allows a global [goto](https://en.wikipedia.org/wiki/GOTO) statement to transfer control out of a nested function and into a previously invoked outer function. This operation requires the stack to be unwound, removing as many stack frames as necessary to restore the proper context to transfer control to the target statement within the enclosing outer function. Similarly, C has the [`setjmp` and `longjmp`](https://en.wikipedia.org/wiki/Setjmp.h) functions that act as non-local gotos. [Common Lisp](https://en.wikipedia.org/wiki/Common_Lisp) allows control of what happens when the stack is unwound by using the `unwind-protect` special operator.

When applying a [continuation](https://en.wikipedia.org/wiki/Continuation), the stack is (logically) unwound and then rewound with the stack of the continuation. This is not the only way to implement continuations; for example, using multiple, explicit stacks, application of a continuation can simply activate its stack and wind a value to be passed. The [Scheme programming language](https://en.wikipedia.org/wiki/Scheme_(programming_language)) allows arbitrary [thunks](https://en.wikipedia.org/wiki/Thunk_(functional_programming)) to be executed in specified points on "unwinding" or "rewinding" of the control stack when a continuation is invoked.

## Inspection[[edit](https://en.wikipedia.org/w/index.php?title=Call_stack&action=edit&section=13)]

See also: [Profiling (computer programming)](https://en.wikipedia.org/wiki/Profiling_(computer_programming))

The call stack can sometimes be inspected as the program is running. Depending on how the program is written and compiled, the information on the stack can be used to determine intermediate values and function call traces. This has been used to generate fine-grained automated tests,[[4\]](https://en.wikipedia.org/wiki/Call_stack#cite_note-4) and in cases like Ruby and Smalltalk, to implement first-class continuations. As an example, the [GNU Debugger](https://en.wikipedia.org/wiki/GNU_Debugger) (GDB) implements interactive inspection of the call stack of a running, but paused, C program.[[5\]](https://en.wikipedia.org/wiki/Call_stack#cite_note-5)

Taking regular-time samples of the call stack can be useful in profiling the performance of programs, because if a subroutine's pointer appears on the call stack sampling data many times, it is likely a code bottleneck and should be inspected for performance problems.

## Security[[edit](https://en.wikipedia.org/w/index.php?title=Call_stack&action=edit&section=14)]

Main article: [Stack buffer overflow](https://en.wikipedia.org/wiki/Stack_buffer_overflow)

In a language with free pointers or non-checked array writes (such as in C), the mixing of control flow data which affects the execution of code (the return addresses or the saved frame pointers) and simple program data (parameters or return values) in a call stack is a security risk, possibly [exploitable](https://en.wikipedia.org/wiki/Exploit_(computer_security)) through [stack buffer overflows](https://en.wikipedia.org/wiki/Stack_buffer_overflow) as the most common type of [buffer overflows](https://en.wikipedia.org/wiki/Buffer_overflow).

One of such attacks involves filling one buffer with arbitrary executable code, and then overflowing the same or some other buffer to overwrite some return address with a value that points directly to the executable code. As a result, when the function returns, the computer executes that code. This kind of an attack can be easily blocked with [W^X](https://en.wikipedia.org/wiki/W%5EX).[*citation needed*] Similar attacks can succeed even with W^X protection enabled, including the [return-to-libc attack](https://en.wikipedia.org/wiki/Return-to-libc_attack) or the attacks coming from [return-oriented programming](https://en.wikipedia.org/wiki/Return-oriented_programming). Various mitigations have been proposed, such as storing arrays in a completely separate location from the return stack, as is the case in the Forth programming language.[[6\]](https://en.wikipedia.org/wiki/Call_stack#cite_note-6)