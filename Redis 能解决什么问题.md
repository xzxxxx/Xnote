https://mp.weixin.qq.com/s?__biz=MzIyNDU2ODA4OQ==&mid=2247484524&idx=1&sn=7ae13d7805f70592245464a7d9512e57&chksm=e80db21adf7a3b0c6b7c8e1367f4c9e772666e1e764a00ad2f4771c87520fdeff68da57f5f07&scene=21#wechat_redirect

## Redis 能解决什么问题

1、缓存，减少了跟数据库的交互，变化频率不高的数据+访问频率高的数据适合缓存

2、Mybatis缓存：1.一级缓存 ，SQLSession ，线程级  ，2. 二级缓存 sqlSessionFactory 进程级

3.存储一些信息

4.数据类型丰富，例如点赞

5.热点事件的排行

6.做消息队列



### Redis中的数据类型

 所有的key 都是String , value的类型有 String，list，set，zset，hash



### 使用缓存

提高性能，保护数据库

### 缓存穿透

查询数据库和缓存中都没有的数据  

解决方法：1、缓存空对象：第一次查询数据库如果查询不到，将null或是一个默认值作为value值存入缓存

只能解决多次访问同一个数据时 ，会导致redis中有大量的null对象

2、布隆过滤器

位图bit

   ### 缓存击穿

缓存中没有，但数据库中有数据（并发访问）：1、没有访问过该条数据, 2、该条数据在缓存中刚好失效

使用分布式锁解决  

 ### 缓存雪崩

1、redis宕机     2、redis中所有数据或者大部分数据都过期，导致访问数据时同一时间大量访问mysql

搭建高可用集群redis      数据的过期时间错开    开启熔断机制

### 缓存与数据库中的数据一致性

   先删除缓存再更新数据库

延时双删

![image-20200723164123058](C:\Users\87634\AppData\Roaming\Typora\typora-user-images\image-20200723164123058.png)







![image-20200724115547029](C:\Users\87634\AppData\Roaming\Typora\typora-user-images\image-20200724115547029.png)

### setnx 设置失败返回0

设置成功则返回ok

 setex 设置过期时间防止 死锁

可能出现业务逻辑执行时间超过 过期时间

![image-20200724122742288](C:\Users\87634\AppData\Roaming\Typora\typora-user-images\image-20200724122742288.png)



value值存线程id

### redis 单线程 一次只能处理一个请求

![image-20200724122906083](C:\Users\87634\AppData\Roaming\Typora\typora-user-images\image-20200724122906083.png)





## Redis 过期删除策略

### 主动删除策略

1.定时删除策略

为每个过期键创建一个定时器，定时器时间到了就删除键

2.定期删除策略

每个一段时间程序检查数据库，删除过期键

### 被动删除策略

1.惰性删除

放任键过期不管，但每次get键时，判断键是否过期，若已经过期则删除

## Redis 淘汰策略

noeviction:返回错误当内存限制达到并且客户端尝试执行会让更多内存被使用的命令（大 部分的写入指令，但 DEL 和几个例外）

 allkeys-lru: 尝试回收最少使用的键（LRU），使得新添加的数据有空间存放。

 volatile-lru: 尝试回收最少使用的键（LRU），但仅限于在过期集合的键,使得新添加的数据 有空间存放。

 allkeys-random: 回收随机的键使得新添加的数据有空间存放。

 volatile-random: 回收随机的键使得新添加的数据有空间存放，但仅限于在过期集合的键。

 volatile-ttl: 回收在过期集合的键，并且优先回收存活时间（TTL）较短的键,使得新添加的 数据有空间存放。

