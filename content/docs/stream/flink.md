# Flink

## 算子

## window
* tumbling windows： 滚动窗口，数据不会重叠，用于统计每分钟的数据
* slinding windows: 时间可能会重叠，用于统计最近几分钟的数据
* session windows：直到固定的时间没有收到元素的时候，窗口关闭

### 划分窗口的方式
* time：按时间划分，数据符合时间范围的在一个窗口里
* count：按数量划分，数据条数到达数量的时候形成一个窗口

### 组合4种基本窗口
* time-tumbling-window 无重叠数据的时间窗口：timeWindow(Time.second(5))
* time-sliding-window 有重叠数据的时间窗口：timeWindow(Time.second(5),Time.second(3))
* count-tumbling-window 无重叠数据的数量窗口：countWindow(5)
* count-sliding-window 有重叠数据的数量窗口：countWindow(5, 3)
  

## WaterMark
解决数据按event time时乱序的问题

allowedLateness(Time.second(2)): 意味着遇到超过时间线2秒的数据来了之后就必须关闭这个窗口了

## checkpoint
### 增量式checkpoint
通过下面方式开启增量式cp
```
env.setStateBackend(new RocksDBStateBackend(checkpointPath, enableIncrementalCheckpointing))
```
增量式cp适合数据量比较大时的checkpoint，缺点是重启恢复比较慢