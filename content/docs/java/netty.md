# Netty

Netty是基于NIO开发的高性能网络通信框架

## java原生NIO有什么不足
编程模型存在bug，编写代码复杂，断线重连，粘包拆包复杂

## Netty核心组件
* ByteBuf字节容器
* Bootstrap和ServerBoostrap启动类
* channel网络操作对象
* EventLoop事件循环
* ChannelHandler消息处理器ChannelPipeline(ChanelHandler链表)

## 什么是拆包粘包
多次发送数据时如果每个数据都太小，tcp层可能将多个数据合在一个tcp包中发送，
如果数据太大，tcp层可能将单个数据包拆成多个tcp包发送

怎么解决？

在每个数据包前面加一个字段表示数据长度，收到包的时候根据这个长度读取够一定量的数据再合成一个数据包。netty框架自带根据长度解决拆包粘包的解码器

## 心跳
客户端每过一定时间会向服务端发送一个ping包，然后服务端回应一个pong包，这样可以确定连接是否真的存活。如果服务端有一定时间没有收到客户端的心跳包就断开客户端的连接

tcp自带保活机制，但是为了灵活和统一处理还是需要在服务端连接层实现心跳包

## Netty零拷贝
netty支持零拷贝技术，可以直接把jvm对外内存映射到堆内，这样减少从堆外拷贝到堆内的操作