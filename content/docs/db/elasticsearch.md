# ElasticSearch

## filter和query区别
* filter是query的一个匹配方式
* query的其他匹配方式比如must是会计算得分的，filter是不会计算得分，以为着filter更快，因为处理更简单
* query的must是不会缓存数据，但是filter是有可能用bit数组缓存的，这样速度又更快

## shard
shard在建立索引的时候就确定了，建立索引之后就不能变更了

增删结点的时候shard会自动负载均衡