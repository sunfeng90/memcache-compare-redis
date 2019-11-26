# MemcacheVsRedis
## memcache是什么？
和Redis类似，可以将数据存储到内存里面，是一种内存Cache。不过memcache不仅仅可以存储普通字符，还可以存储图片和视频等等。
## memcache与redis的区别
Redis|Memmcache
:-|:-
String、list、set、sorted和hash|string
每个K最大存储量为1G|每个K最大存储量为1M
支持持久化(RDB和AOF)|不支持持久化
## Memcached的特点
 - 协议简单 
 - 基于libevent的事件处理 
 - 内置内存存储方式 
 - memcached不互相通信的分布式
## Libevent原理简介
## memcachehe和Redis过期键的策略
 ### Redis过期键的删除策略
  - 惰性删除
   1. 在哪里实现？db.c/expireIfNeeded
   2. 如何实现的？
    - 执行读写命令的时候，例如GET、SADD或者HGET；
    - 调用expireIfNeeded：
      1) 获取键的过期时间，如果没有过期时间，那么什么都不做，直接退出；
      2) 如果服务器正在载入，也是什么都不做，直接退出；
      3) 如果过期时间比当前时间小，说明没有过期，也是什么都不做，直接退出；
      4) 删除key，接着像AOF文件或者其他附属子节点传播该过期信息，然后发送事件通知；
      5) 最后删除该key.    
  - 定期删除
   1. 在哪里实现？redis.c/activeExpireCycle
   2. 如何实现的？
    - 数据库周期性的操作redis.c/serverCron时，就执行activeExpireCycle函数分多次遍历多个数据库，从而删除过期键.
      - 设置默认每次检查数据库的数量为16，默认每个数据库检查键的数量为20;
      - 如果实际数量比默认值小，那么就以默认值为准;
 ### memcache过期键的删除策略
 ### 如果键已经过期了，Redis和memcache访问键获取的结果有什么不同？
  - memchache. 返回键对应的值然后删除该键和它对应的值。
  - Redis. 返回空然后删除该键和它对应的值
 - 惰性过期算法(lazy expiration)
 - 最近最少使用算法(LRU：Least Recently Used)
## memcache适用场景
  - 对持久化或者数据结构要求不高的场景；
  - 存储的Key/Value大小不是非常大，毕竟Value最大是1M；
  - 单纯缓存，对可靠性要求不高；
  - 访问比较频繁的数据，安全性差的数据，丢失无所谓的数据,例如Token;
  - 数据更新，比较频繁的数据，比如用户的在线状态或者下线状态;

[参考一](https://www.cnblogs.com/JavaBlackHole/p/7726195.html)
[node-memcached](https://github.com/elbart/node-memcache#readme)
[memcached](https://github.com/memcached/memcached)
[参考二](https://www.jianshu.com/p/b6a710a01a6a)