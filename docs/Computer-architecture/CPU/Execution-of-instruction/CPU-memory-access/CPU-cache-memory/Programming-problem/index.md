# Programming problem

本章总结在进行concurrent programming的时候，会遇到的和cache相关的问题。



## stackoverflow [Are cache-line-ping-pong and false sharing the same?](https://stackoverflow.com/questions/30684974/are-cache-line-ping-pong-and-false-sharing-the-same)



[A](https://stackoverflow.com/a/30687047/10173843)

> NOTE: 
>
> 总结得非常好

### Summary:

*False sharing* and *cache-line ping-ponging* are related but not the same thing. False sharing can cause cache-line ping-ponging, but it is not the only possible cause since cache-line ping-ponging can also be caused by true sharing.