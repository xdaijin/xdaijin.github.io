---
title: "Jvm排障指南"
summary: "这篇文章可以指导如何排查jvm问题"
keywords: "jvm,troubleshooting"

date: 2025-11-24T21:38:40+08:00
lastmod: 2025-11-24T21:38:40+08:00

math: false
mermaid: false

categories:
  - Java
tags:
  - jvm
  - troubleshooting
---

## cpu问题

cpu高一般有两种原因：

- gc
- 压缩解压缩，序列化反序列化，for循环很大且没有io

通过查看哪个线程占用cpu高来区分是什么原因导致的cpu高

```bash
ps -Hp <pid>
```

gc线程一眼就能看出来，如果是非gc线程需要进一步查看

先将十进制线程号转换为十六进制
  
```bash
printf '%x\n' <pid>
```

然后使用jstack看具体是哪个线程占用cpu高，并从线程堆栈中分析分析具体原因
  
```bash
jstat <pid> | grep <十六进制线程号> -C5
```

## 内存问题

内存问题分析比较复杂，jvm内存分为三块：

- 堆内内存
- jvm使用的堆外内存（native）
- 非jvm使用的堆外内存（native）

### 查看堆内内存

查看堆内内存概览:

```bash
jcmd <pid> GC.heap_info
```

查看类大小统计直方图：

```bash
只查看存活对象
jcmd <pid> GC.class_histogram | head -n 20
或查看所有对象
jcmd <pid> GC.class_histogram -all | head -n 20
```

查看堆内内存详情：

```bash
只dump存活对象
jcmd <pid> GC.heap_dump <filename>
或dump所有对象
jcmd <pid> GC.heap_dump -all <filename>
```

使用visualvm分析dump文件

查看堆内内存和jvm使用的堆外内存(需要加启动参数-XX:NativeMemoryTracking=detail)：

```bash
jcmd <pid> VM.native_memory detail scale=mb
```

查看所有jvm内存：

```bash
pmap -x <pid>
```

## 参考

参考https://developer.aliyun.com/article/1470395
