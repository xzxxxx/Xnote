# ZooKeeper ：文件性数据库

## zookeeper解决了什么问题

1、命名服务
2、配置管理
3、集群管理
4、分布式锁
5、队列管管

数据一致性

## zookeeper 数据结构

树形

持久化有序节点:创建业务中唯一id

持久化无序节点

临时有序节点：生成分布式锁

临时无序节点

内存型



## watcher

一次性的

 ## zab投票机制

### 恢复模式选出leader

第一轮投票都投自己，多台服务器间进行通信 重新投票

然后服务器刚启动时则服务器id大的获得投票，事务czXid大的获得投票

### 广播模式

 （1）Client通过某个follower请求写操作时，该follower会把这个请求发给leader；

（2）leader再将这个更新（proposal），顺序发送给follower；

（3）follower收到leader更新时，follower会将数据持久化到磁盘；

（4）当follower写到磁盘后，就会向leader发送ACK；

（5）==当leader收到半数以上的follower向它发送ACK后，就向全部的follower发送commit==；

（6）leader并在本地commit消息（commit的意思是就是这个消息可对外读了）；

（7）当follower收到leader发送的commit后，就会将磁盘里的数据写进内存数据库，同时commit（每个follower都有内存数据库，Client去向follower请求数据时，都是通过内存数据库读取的）。同时每条消息，都有一个递增的id。

