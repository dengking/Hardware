# lrucache

一、看了一下两个library的实现，原理和 labuladong [算法题就像搭乐高：手把手带你拆解 LRU 算法](https://mp.weixin.qq.com/s/b0YVCccJ8mFP6lI-1NiQOQ) 中的是一致的，即hash map + double linked list

二、下面的两个实现的差异在于

1、[mohaps](https://github.com/mohaps)/[lrucache](https://github.com/mohaps/lrucache) 中，使用了custom doublelinked list；[mohaps](https://github.com/mohaps)/[lrucache11](https://github.com/mohaps/lrucache11) 中使用的是`std::list`



## github [mohaps](https://github.com/mohaps)/[lrucache](https://github.com/mohaps/lrucache)





## github [mohaps](https://github.com/mohaps)/[lrucache11](https://github.com/mohaps/lrucache11)