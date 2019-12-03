# MemcacheVsRedis
## Memcache是什么？
和Redis类似，可以将数据存储到内存里面，是一种内存Cache。不过memcache不仅仅可以存储普通字符，还可以存储图片和视频等等。
## Memcache和Memcached的区别
- Memcache是一个自由和开放源代码、高性能、分配的内存对象缓存系统。用于减轻数据库负载。
- Memcached是该系统的项目名称，Memcached是该系统的主程序文件。这里的d的英文是daemon(守护进程)，就是常驻进程的意思。
  以守护程序方式运行在一个或多个服务器中，随时接受客户端的连接操作，使用共享内存存取数据。
## Memcached的特点
 - 协议简单:使用简单的基于文本的协议；
 - 基于libevent的事件处理

    1）libevent是一个程序库，他将Linux的epoll、BSD类操作系统的kqueue等时间处理功能封装成统一的接口，memcached使用libevent从而在Linux、BSD、Solaris等操作系统
     发挥高性能；
    2）说到epoll和kqueue，这里和NodeJs联系起来讲一下。提起NodeJs的特点，人们常常说，它是基于事件驱动，异步且非阻塞的。这里的异步非阻塞就和epoll和kqueue有关系；
    3）与异步相对应的是同步，同步会造成阻塞，从而导致CPU得不到高效的使用。内核在进行文件I/O操作时，通过文件描述符进行管理；
    4）内核在进行文件I/O操作时，通过文件描述符进行管理。应用程序如果需要进行I/O调用，必须先打开文件描述符，然后再根据文件描述符去实现文件的读写。
    阻塞I/O完成整个获取数据的过程，而非阻塞I/O不带数据直接返回，要获取数据，需要通过文件描述符进行获取。
    非阻塞I/O返回的调用的状态，不是业务层需要的数据，所以通过轮训来判断是否完成。
    - read
    - select
    - poll
    - epoll
    - kqueue
    ![avatar](./kqueue.png)
 - 内置内存存储方式 
 - memcached是不互相通信的分布式
 2. Memcached如何实现分布式
![avatar](./memcached分布式.png)
## Memcached的访问模型
![avatar](./memcached访问模型.png)
 1. 应用程序输入需要写缓存的数据，例如Token；
 2. Memcache API输入路由算法模块，路由算法根据Key和Memcache集群服务列表得到一台服务器编号；
 3. 由服务器编号得到Memcache及对应的IP地址和端口号；
 4. API调用通信模块和指定编号的服务器通信，将数据写入该服务器，完成一次分布式缓存的写操作。
## Memcached内存分配策略-slab 
一个内存分配算法要考虑算法的效率，管理内存所占的空间和内存碎片的问题。
slab能较好的规避内存碎片的问题，但也带来了一定的内存浪费，算法的效率还不错。
## 数据类型
  ### Redis支持String、List、Set、Sorted和Hash
  ### Memcache支持String
## 持久化
  ### Redis支持RDB和AOF的持久化
  ### Memcache不支持持久化
## 大小
  ### Redis的K最大存储量为1G
  ### Memcache最大存储量为1M
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
## 实操
  - 过期键取值
## memcache适用场景
  - 对持久化或者数据结构要求不高的场景；
  - 存储的Key/Value大小不是非常大，毕竟Value最大是1M；
  - 单纯缓存，对可靠性要求不高；
  - 访问比较频繁的数据，安全性差的数据，丢失无所谓的数据,例如Token;
  - 数据更新，比较频繁的数据，比如用户的在线状态或者下线状态;

## 资料
  [Memcache官网](https://memcached.org/)
- [参考一](https://www.cnblogs.com/JavaBlackHole/p/7726195.html)
- [node-memcached](https://github.com/elbart/node-memcache#readme)
- [memcached](https://github.com/memcached/memcached)
- [参考二](https://www.jianshu.com/p/b6a710a01a6a)