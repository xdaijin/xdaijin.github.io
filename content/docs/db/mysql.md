# MySql

sql数据库主要用于misson-critical系统，就是那种7x24小时都要正常运行的系统

## 事务四大特性ACID
- 原子性(Atomicity)  
指一个事务内的所有操作要么全部成功，要么全部失败回滚，涉及数据库的autocommit、commit和rollback

- 一致性(Consistency)  
指数据库事务执行前后数据库的状态要保持一致，主要指数据存储过程中出现故障后如何保证数据的正确性，涉及doublewrite buffer和crash recovery

- 隔离性(Isolation)  
指不同事务之间互不影响，涉及到数据库的事务，涉及到事务的隔离性以及数据库的锁

- 持久性(Durability)  
指数据库的操作一旦完成后就是永久，就算重启数据库挂掉或者重启都不会丢失，涉及到doublewrite buffer和binlog

## InnoDB的多版本
mvcc是事务能够实现隔离以及回滚的原因

## InnoDB的架构
![InnoDB架构图](/images/5.png)

## InnoDB内存结构
### Buffer Pool
Buffer Pool用来缓存表数据和索引数据，缓存清除算法用的LRU(least recently used)算法。

![Buffer Pool](/images/6.png)

LRU算法实现原理：
- 数据page用链表串联起来，链表上面是常用数据，链表下面是不常用数据
- 从硬盘读到内存中的page直接插入到链表中间
- 读取缓存中的不常用数据后，将不常用数据挪到链表头成为常用数据
- 每次清除数据就从链表下面的不常用数据里面清除

### Change Buffer
Change Buffer是用来存储那些发生了变化又不在Buffer Pool里面的二级索引的。二级索引不像聚簇索引那样集中，每次修改的位置是随机的，如果每次修改都去修改磁盘并加载到Buffer Pool里面会导致性能很低，所以现在Change Buffer里面改，等到系统空闲的时候再批量更新到磁盘中

### Adaptive Hash Index
自适应hash索引就是个提速技术，用来给经常访问的数据用的，主要给查询用的，一般是索引的前缀来建立hash索引

### Log Buffer
Log Buffer用来存储要写到磁盘上的log文件，一个很大的Log Buffer让你能同时执行很多事务且在事务commit之前都不需要把redo log写到磁盘。如果你有很多事务要插入更新删除很多行数据，那就把Log Buffer设大一点

## InnoDB磁盘结构
### InnoDB Indexes
#### Clustered index
InnoDB的每个表都有一个特殊的索引叫聚簇索引，一般来说聚簇索引就是主键索引。聚簇索引的实现是b+树，树的叶子节点存储整行数据

> 聚簇索引的本质就是数据和索引存在一起，这样可以减少索引和数据分开存储导致的磁盘io上升

聚簇索引是自动建立的不需要用户定义。

聚簇索引建立的逻辑：
1. 如果有主键，使用主键建立聚簇索引
2. 如果没有主键，选择第一列非空唯一索引建立聚簇索引
3. 如果没有主键和非空唯一索引，就会用rowid建立聚簇索引

#### Secondary index
二级索引存的值是主键的值

### Doublewrite Buffer
直接把更新的page写到磁盘上正确的位置是离散和缓慢的，这过程中容易发生故障。先把更新的page一整块写到doublewrite的一个文件上，这过程比较快时间比较短，不容易出错。在最终写入的过程中如果发生故障可以从这个doublewrite文件中恢复。这样加强了mysql的故障恢复能力

### Redo Log
用于故障恢复

### Undo Log
用于事务回退以及其他需要看到原始数据的事务

## 故障恢复
1. tablespace discovery  
先确定需要执行redo的表空间

2. redo log application  
执行redolog

3. roll back incompleter transaction  
使用undo log回退没完成的事务

4. change buffer merge  
合并chage buffer里面的二级索引

5. purge  
删除被标记删掉的数据

## InnoDB的锁和事务模型
### row locks
锁的类型分两种，共享锁和排他锁

加锁方式
```
select ... for update
insert into ... values ...
update ... where ...
delete ... where ...
```

锁的影响范围分为record lock，gap lock，next-key lock

### 隔离级别
- repeatable read  
    默认隔离级别，在不加锁情况下，整个事务内读到的数据都是第一次读时产生的快照。会出现幻读

    在加锁情况下会根据查询条件用不同的锁，如果是where a = b的查询条件就加record lock，如果是范围的查询条件就加gap lock或者next-key lock

- read commited  
    在不加锁的情况下读到的是其他事务提交的最新数据。会出现不可重复读和幻读

    在加锁的情况下只会加record lock，不会用gap lock，所以在其中事务可能会插入新的数据，导致第二次读范围数据的时候出现幻读

- read uncommited  
    不加锁查询的时候，一个事务未提交时，其他事务能看到它修改的数据。会出现脏读、不可重复读、幻读

- serializable  
    和repeatable read类似，不同在于所有查询都会强制上共享锁

脏读: 事务里读取一个数据后，改数据被回滚了，这个数据就变成脏数据了

不可重复读：在事务里读取了一个数据，另一个事务修改了改数据并提交了，导致这个事务两次读取的数据不一致 

幻读：事务里读取一个范围里的数据后，另一个事务在这个范围里新增了数据并提交了，导致这个事务第二次读的时候多了几条数据

## 分库分表
首先定义垂直和水平
> 垂直：垂直分出来的结构是不一样的，比如把用户表和订单表分到两个库，把同一表中的两列分到两个表中

> 水平：水平分出来的结构还是一样的，只是内容不一样，比如用户信息根据地方进行分库或者分表

一般优先垂直分，再水平分

> 数据量过大之后的两项解决方案，一是分库分表，二是冷热分离

## 事务
事务是默认每一次dml操作都会自动提交一次，如果需要扩大事务范围，需要收到那个开启事务

## 回表
如果查询使用的不是聚簇索引，那么查到的是索引对应的值其实是聚簇索引的值，然后还要再根据这个值去查询数据

## 死锁
mysql也会出现死锁，解决死锁的办法要么手动关闭某个事务，要么保证锁资源的顺序一致

## limit问题
如果limit的offset很大的话，会把offset前面的数据全部加载出来再扫描到offset位置，浪费内存，影响缓存，解决办法是找到offset的位置从这位置开始再limit

