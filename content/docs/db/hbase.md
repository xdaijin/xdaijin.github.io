# Hbase

基于hadoop的非关系，分布式强一致性数据库管理系统。

## hbase架构
* Master
    监控和管理所有RegionServers，metadata变化的接口，运行在hdfs的namenode
* RegionServer
    管理和提供所有region的服务，运行在datanode上

## 数据存储结构
```
Table                    (HBase table)
    Region               (Regions for the table)
        Store            (Store per ColumnFamily for each Region for the table)
            MemStore     (MemStore for each Store for each Region for the table)
            StoreFile    (StoreFiles for each Store for each Region for the table)
                Block    (Blocks within a StoreFile within a Store for each Region for the table)
```

## RowKey设计
数据按rowkey存储，所以要避免rowkey集中在一台服务上形成热点问题，通过以下方法处理

### Salting
加盐，在rowkey前面加随机数据
例如
```
foo0001
foo0002
foo0003
foo0004
```
加盐后
```
a-foo0003
b-foo0001
c-foo0004
d-foo0002
```

### Hashing
用哈希值

### reversing the key
反转时间，这样最近的数据在最前面

## 数据模型
* NameSpace
    含有多个表
* Table
    含有多行数据
* Row
    含有多列数据
* Column Family
    具有相同前缀的列
* Cells
    单个列数据