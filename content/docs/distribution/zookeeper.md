# Zookeeper

## 简介

## 两个模式
* 崩溃恢复
* 原子广播

## 三个阶段
* 选举
* 恢复
* 广播
  
## 三个状态
* Leading
* Following
* Looking
* Observing(特殊状态)：observer不参与选举，可认为和Zab没关系
  
## 保证两个问题
* 已经被leader提交的事务最终被所有follower commit
* 丢弃被leader提出但是还没有commit的事务