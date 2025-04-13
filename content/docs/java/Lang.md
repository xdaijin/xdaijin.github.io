---
title: Lang
type: docs
weight: 1
---
## 关键字

## 集合

## 锁

## 多线程

## io

## Stream
stream从操作分为中间操作和结束操作，中间操作分为statelessOp和statefulOp，结束操作就是terminalOp

然后从载体分为Stream和Sink，Stream只是定义了操作和数据源头，Sink才是数据真正操作的地方，因为执行的过程是调了Iterator.forEachRemaining,所有Stream的内容必须是Iterable

stateless和stateful区别：stateless操作就是一直调sink.accept直到stateful操作就行了，而stateful要用list或者map之类的来存储中间结果。

Stream在遇到terminalOp之前都是构建Stream的双向链接，遇到terminalOp就开始溯源并生成sink链，直到源头就开始执行sink.begin, iterator.forEachRemaing(sink.accept), sink.end最后生成结果

stream在idea里的调试有个跟踪当前流链的调试

## FAQ