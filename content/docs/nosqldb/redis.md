# Redis

‌Redis是一个_高性能_的_键值对_内存数据库，它支持_丰富数据结构_，如字符串、哈希表、列表、集合和有序集合等。Redis的主要特性包括其基于内存的操作、支持‌持久化、事务处理、发布/订阅功能等。

## 使用场景
* 缓存
* session管理
* 全局id
* 计数器
* 限流
* 位统计
* 购物车
* 消息队列
* 抽奖
* 点赞、签到、打卡
* 商品标签
* 商品筛选
* 用户关注、推荐模型
* 排行榜

## 数据结构
* Strings: int和shtring，用于缓存，或计数器，还可以进行位操作，值最大可用512mb

* Hash: ziplist和hashtable，用于事件追踪，反欺诈，用户session管理，活跃session追踪
  
* List: ziplist和linked list，两端插入快，读取慢，用来实现队列或者栈，如果需要中段查询的就不要用list，可用用sorted set
  
* Set: intset和hashtable，用来追踪不重复的东西，可以做交集，差集
  
* Sorted set: ziplist和skiplist，用来做排行榜，限流
  
* Stream: append-only log，用于事件源，传感器监控，通知
  
* Bitmap: 不是真正的数据类型，是在string上的操作，用来表示数字的集合，或者像文件系统的权限表示
  
* Bitfield

* Geospatial: 用来寻找半径范围内的点，常用来寻找范围的单车

## 持久化策略
* RDB(Redis database)
    通过fork一个子进程，利用copy on write的技术来复制redis内存里的数据，数据时间有间隔，恢复速度快
* AOF(Append only file)
    保存了完整的数据，但是aof文件会越来越大，恢复速度比rdb慢

## 过期策略

The following policies are available:

* **noeviction**: New values aren’t saved when memory limit is reached. When a database uses replication, this applies to the primary database
* **allkeys-lru**: Keeps most recently used keys; removes least recently used (LRU) keys
* **allkeys-lfu**: Keeps frequently used keys; removes least frequently used (LFU) keys
* **volatile-lru**: Removes least recently used keys with the `expire` field set to `true`.
* **volatile-lfu**: Removes least frequently used keys with the `expire` field set to `true`.
* **allkeys-random**: Randomly removes keys to make space for the new data added.
* **volatile-random**: Randomly removes keys with `expire` field set to `true`.
* **volatile-ttl**: Removes keys with `expire` field set to `true` and the shortest remaining time-to-live (TTL) value.

It is important to understand that the eviction process works like this:

* A client runs a new command, resulting in more data added.
* Redis checks the memory usage, and if it is greater than the `maxmemory` limit , it evicts keys according to the policy.
* A new command is executed, and so forth.

redis的lru不是严格的lru，因为筛选淘汰key代价太大，redis通过在一组key里面选择符合lru的删除

## 高可用方案
* 主从复制: 需要手动切换，后者用哨兵切换
* 哨兵模式: 启动切换，但是不支持数据分区，集群部署复杂
* 集群模式: 分布式存储

## 缓存问题
* 缓存穿透: 数据既不在数据库，也不在redis
* 缓存击穿: 数据缓存过期
* 缓存雪崩: 数据大量过期或者redis故障

## 为什么性能高

## 如何保持缓存和数据库一致
* 先删除缓存，再更新数据库
* 先更新数据库，再删除缓存
* 普通双删
* 延迟双删

我的办法，先设置一个短期的超时时间，更新数据库后再删除数据，这样可以避免在更新数据库后报错，导致没有更新缓存

## redis事务机制
redis的事物没有回滚机制，只是一组命令按顺序执行完，且忽略报错

## 如何实现分布式锁
* setnx + setex
* setnx(key, value, nx, px)

## 如何做缓存预热
后台做个页面点击热门数据

## 用redis为什么还要在客户端进一步缓存
缩短响应时长，减少网络开销，在有部分数据过于热门的时候效果很好


