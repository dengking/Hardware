# 关于本章

本章讨论memory access instruction，其中非常重要的一个内容是: atomic，这将在`./Atomic`章节中进行讨论。

## Memory access instruction是否支持unaligned address？



### Intel

X86 MOV 是支持 unalignend address的，但是无法保证atomic；

### ARM 

从下面链接中的内容来看，ARM是可以配置的。

1、https://developer.arm.com/documentation/dui0472/m/compiler-command-line-options/--unaligned-access----no-unaligned-access

2、stackoverflow [Take advantage of ARM unaligned memory access while writing clean C code](https://stackoverflow.com/questions/32062894/take-advantage-of-arm-unaligned-memory-access-while-writing-clean-c-code)

