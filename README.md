# MemcacheVsRedis
## Memcache是什么？
和Redis类似，可以将数据存储到内存里面，是一种内存Cache。不过memcache不仅仅可以存储普通字符，还可以存储图片和视频等等。
## Memcache与Redis的区别
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
## memcache适用场景
  - 对持久化或者数据结构要求不高的场景，可以适用memecache；
  - 存储的Key/Value大小不是非常大，毕竟Value最大是1M；
  - 单纯缓存，对可靠性要求不高；

[参考](https://www.cnblogs.com/JavaBlackHole/p/7726195.html)