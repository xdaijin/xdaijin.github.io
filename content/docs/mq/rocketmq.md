# RocketMq

Apache RocketMQ 自诞生以来，因其架构简单、业务功能丰富、具备极强可扩展性等特点被众多企业开发者以及云厂商广泛采用。历经十余年的大规模场景打磨，RocketMQ 已经成为业内共识的金融级可靠业务消息首选方案，被广泛应用于互联网、大数据、移动互联网、物联网等领域的业务场景。

## 使用场景
* 异步通信
* 社交网络活动流
* 数据管道
* 贸易流程
* 分布式事务

## 角色构成
* producer
* broker
* consumer

## 领域模型
* topic
* message queue
* message
* producer
* consumer group
* consumer
* subscription

## 消息类型
* normal
* fifo
* delay
* transaction

## 如何负载均衡
* 消息粒度负载均衡
* 队列力度负载均衡

## 如何保证消息不丢失
得分别从producer、broker、consumer三端来保证

