# cache2j 是一款轻量级的进程内缓存框架
`cache2j`是一款超级轻量的进程内缓存框架，基于java语言实现，底层存储基于`ConcurrentHashMap`,核心功能包括：  
1、提供了基于K,V形式的对象存储/查询能力；  
2、支持多种类型的缓存淘汰策略，包括基于ttl（过期时间）方式和cap（容量）方式，默认基于ttl，根据写入时间FIFO淘汰；  
3、提供了缓存监听器供用户实现并注册（缓存被淘汰时会回调监听器的`callback`方法）；  
4、支持缓存过期自动加载/更新机制（实现`CacheLoader`）；  
5、支持缓存命中率相关统计（`cacheBuilder.stats()`）；  
6、支持持久化，刷盘方式为异步（开发中）；

`cache2j` VS `guava cache`：  
1、底层实现：  
`guava cache`底层所用的是拓展的`HashTable`，和`ConcurrentHashMap`十分相似，通过引入一个`Segment`数组，对`HashTable`进行分段，通过分离锁、`final`以及`volatile`的配合，实现了并发环境下的线程安全;缺点：底层实现采用的是jdk1.8以前`ConcurrentHashMap`的实现，性能没有得到优化。  
`cache2j`底层采用`jdk`自带的`ConcurrentHashMap`；底层设计跟着`jdk`版本走，性能上可以得到优化（1.8版本以后取消了锁分段技术并引入了红黑树，性能得到极大的优化）。  

2、淘汰策略实现  
`guava cache`并未引入任何独立线程执行缓存“清理”工作，而是在读写的时候“顺便”做了部分清理工作。  
`cache2j`如果设置了`Monitor`（执行淘汰任务的工作线程），则由一个守护线程定期负责清理工作；如果未指定任何`Monitor`,则在`put`操作时完成少部分的清理工作。  

整体而言，`cache2j`无论从性能和使用灵活性上都十分不错！  

[wiki地址](https://github.com/zhangxiaomin1993/cache2j/wiki)  

PS：cache2j是本人基于业余兴趣开发，代码一直在更新，目前并未做任何压力测试（未来会补充），请勿做商业用途，仅供学习交流参考！谢谢支持！  
contact me:1261608273@qq.com
