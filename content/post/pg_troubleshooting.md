---
title: "Pg排障指南"
summary: "这篇文章介绍如何进行pg数据库问题排查"
keywords: "postgres,troubleshooting"

date: 2025-12-06T00:14:55+08:00
lastmod: 2025-12-06T00:14:55+08:00

math: false
mermaid: false

categories:
  - database
tags:
  - postgres
  - troubleshooting
---

## 了解pg_stat_activity视图

pg_stat_activity每行显示一个进程和这个进程的活动信息

主要列：

- pid 进程号
- client_addr 客户端地址
- backend_start 进程开始时间，就是客户端连接时间
- xact_start 进程的当前事务开始时间，如果当前没有事务，值为null，如果当前的查询是事务中的第一个查询，那xact_stat等于query_start
- query_start 当前活动中的查询的开始时间，如果当前状态不是active，那就是上一个查询的开始时间
- state_change 上一次状态改变时间
- wait_event_type 等待时间类型
  - Activity
  - BufferPin
  - Client
  - Extension
  - InjectionPoint
  - IO
  - IPC
  - Lock 进程在等待重量级锁
  - LWLock 进场在等待轻量级锁
  - Timeout
- state 状态
  - starting 进程初始化
  - active 正在执行查询
  - idle 进程在等待新指令
  - idle in transaction 进程在事务中，但是没有执行查询
  - idle in transaction (aborted)
  - fastpath function call
  - disabled
- backend_xid 当前进程的事务id
- backend_xmin
- query 查询语句
