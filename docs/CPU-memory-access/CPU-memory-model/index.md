# 关于本章



## 内容概述

Compiler，CPU执行memory reordering的目的是: optimization，显然它是遵循optimization principle的；

在进行lockless programming的时候，由于memory reordering的存在，导致了unordering、不确定性，进而导致uncomputational，显然为了make it computational，我们需要添加control: ordering，通过memory barrier/fence来实现ordering；

在preshing的lockfree programming系列文章中已经进行了非常好的总结，因此本章只是进行梳理，基本上没有深入的内容；



## 章节说明

本章首先介绍一个programmer平时不会发现、但是普遍存在的的memory reordering，以及它所带来的影响；然后正式介绍memory ordering；然后介绍control手段: memory barrier；

