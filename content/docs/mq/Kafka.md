---
title: Kafka
type: docs
weight: 2
---

Pages can be organized into folders.
## Exactly Once
* 仅发送一次：单分区仅发送一次由生产者幂等性保证，多分区仅发送一次由事务机制保证
* 仅消费一次：kafka通过消费点的提交来控制消费进度，而消费点的提交被抽象成向系统topic发送消息。然后消费者需要通过幂等性或者二次提交来确保消息不会重复消费

## 生产者幂等性
在创建Kafka生产者的时候设置enable.idempotence 参数，用于开启生产者幂等性。
```
val props = new Properties()
props.put(ProducerConfig.ENABLE_IDEMPOTENCE_CONFIG, "true")

val producer = new KafkaProducer(props)
```
Kafka 的发送幂等是通过序列号来实现的，每个消息都会被分配一个序列号，序列号是递增的，这样就可以保证消息的顺序性。当生产者发送消息时，会将消息的序列号和消息内容一起写入到日志文件中，下次收到非预期序列号的消息就会返回 OutOfOrderSequenceException 异常。
设置 enable.idempotence 参数后，生产者会检查以下三个参数的值是否合法（ProducerConfig#postProcessAndValidateIdempotenceConfigs）
ꔷ max.in.flight.requests.per.connection 必须小于 5
ꔷ retries 必须大于 0
ꔷ acks 必须设置为 all

## 事务消息流程
客户端开启事务代码
```
// 事务初始化
val props = new Properties()
...
props.put(ProducerConfig.TRANSACTIONAL_ID_CONFIG, transactionalId)
props.put(ProducerConfig.ENABLE_IDEMPOTENCE_CONFIG, "true")

val producer = new KafkaProducer(props)
producer.initTransactions()
producer.beginTransaction()

// 消息发送
producer.send(RecordUtils.create(topic1, partition1, "message1"))
producer.send(RecordUtils.create(topic2, partition2, "message2"))

// 事务提交或回滚
producer.commitTransaction()
```