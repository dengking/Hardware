# 关于本章

本章标题"instruction sequence"的含义是指令序列，它是指通过特定的指令序列来实现concurrency control的方式。

## 实现思路

依据multiple model，我们可知，对memory的operation可以分为两大类:

1、read

2、write

atomic operation的实现思路是典型的"assemble as atomic primitive": 将可能的read、write 操作进行**集成**，提供实现**集成功能**的atomic instruction，典型的例子包括:

- Test-and-set
- Fetch-and-add
- Compare-and-swap