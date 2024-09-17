# Disruptor

一个高性能无锁队列，解决多线程之间内存队列的延迟

## 基本概念
* RingBuffer
* Sequence
* SequenceBarrier
* EventProcessor
* EventHandler
* Producer
* Wait Strategy

## wait strategy
* BlockingWaitStrategy
* SleepingWaitStrategy
* YieldingWaitStrategy
* BusySpinWaitStrategy
* PhasedBackoffWaitStrategy

## 设计原理
* 环形数组
    避免垃圾回收，采用数组而非链表。同时数组对缓存更友好
* 元素位置定位
    数组长度2^n，通过位运算，加快定位的速度。下标采取递增的形式。不用担心index溢出的问题。index是long类型，即使100万QPS的处理速度，也需要30万年才能用完。
* 无锁设计
    每个生产者或者消费者线程，会先申请可以操作的元素在数组中的位置，申请到之后，直接在该位置写入或者读取数据。整个过程通过原子变量CAS，保证操作的线程安全。

## 多生产者写数据
多生产者些数据的时候可能会导致中间有部分位置还没有写入成功，通过增长和ringbuffer一样大小的availblebuffer来判断这个位置是否已经写入数据