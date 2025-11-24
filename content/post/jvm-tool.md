---
title: "Jvm Tool"
summary: "这篇文章介绍了常用的jvm工具"
keywords: "jvm,tool"

date: 2025-11-24T22:39:49+08:00
lastmod: 2025-11-24T22:39:49+08:00

math: false
mermaid: false

categories:
  - Java
tags:
  - tool
---

## 查看java进程

```bash
jcmd
```

或者

```bash
jps
```

## 查看线程堆栈

```bash
jcmd <pid> Thread.print
或
jstack pid
```

## 查看堆内存信息

```bash
jcmd <pid> GC.heap_info
```

## dump堆内存信息

dump存活对象：

```bash
jcmd <pid> GC.heap_dump filename
或
jmap -dump:live,file=filename <pid>

```

dump所有对象：

```bash
jcmd pid GC.heap_dump -all filename
或
jmap -dump:all,file=filename <pid>
```

## 查看class大小直方图

```bash
查看GC后存活对象
jcmd <pid> GC.class_histogram
或者
jmap -histo:live,file=filename <pid>

显示前十
jcmd <pid> GC.class_histogram | head -n 10
或者
jmap -histo:live <pid> | head -n 10

查看所有对象
jcmd <pid> -all GC.class_histogram
或者
jmap -histo:all,file=filename <pid>
```

## 查看和jvm配置

```bash
jcmd pid VM.flags
```

## 查看native内存

启动java进程的时候需要加上参数:
> -XX:NativeMemoryTracking=detail

执行以下命令查看NMT内存:

```bash
jcmd <pid> VM.native_memory detail scale=mb
```

## 查看GC信息

下面的命令每5秒一次，打印10次gc信息

```bash
jstat --gcutil 5000 10
```

## 查看JVM参数

```bash
ps -ef |grep java
```
