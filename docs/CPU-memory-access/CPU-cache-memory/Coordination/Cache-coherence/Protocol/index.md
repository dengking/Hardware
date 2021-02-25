# 关于本章

本章讨论各种一些[cache coherency](https://en.wikipedia.org/wiki/Cache_coherency) protocol：

| protocols      | states                                                       | 章节 |
| -------------- | ------------------------------------------------------------ | ---- |
| MSI protocol   | **M**odified、**S**hared、**I**nvalid                        |      |
| MESI protocol  | **M**odified、**E**xclusive、**S**hared、**I**nvalid         |      |
| MOESI protocol | **M**odified、**O**wned、**E**xclusive、**S**hared、**I**nvalid |      |

可以看到，这些protocol的差异主要在于state数的不同。