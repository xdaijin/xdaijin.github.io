# 谢代锦
Tel: 18516761018  Email: xdaijin@outlook.com

## 个人简介
1. 熟悉多线程开发
2. 有不少线上排障调优经验
3. 对高并发系统架构设计有一定的经验
4. 对分布式系统设计理念有一定了解

## 工作经历
### 个人兼职                                                2021-07 - 至今
#### 移动审计工作数据支持
项目介绍: 移动内部审计工作
技术栈: oracle, mysql  
主要工作:  
1. 配合审计工作进行oracle数据查询统计工作
2. 利用mysql导入清单数据进行数据分析工作

#### 停车场管理系统
项目介绍: 支持收费,vip包月和远程开闸的停车场管理系统
技术栈: java,springmvc,netty,mybatis,mysql
主要工作:  
1. 基于netty实现服务器代码
2. 基于netty提供客户端代码
3. 实现vip包月购买功能

#### 报表分析
项目介绍: 基于数据库的定时报表分析
技术栈: java,springmvc,mybatis,mysql,crontab  
主要工作:  
1. 根据业务需求编写job查询数据库进行数据统计,配置crontab每天执行数据统计
2. 统计结果保存到mysql中
3. 提供报表查询查询接口

### 哈啰出行    技术专家                                     2015.08 - 2021.06
#### owl系统
项目介绍: 基于jager, micrometer实现的集链路追踪,日志查询,服务指标监控告警三位一体的系统  
技术栈: java,springmvc,opentracing,micrometer,kafka,flink,es,opentsdb  
主要工作:  
1. 系统调研和开原方案选型
2. 数据收集agent开发
3. 配合中间进行tracing和metric数据埋点工作
4. tracing和metric数据导出器开发
5. 通过flink job进行数据清洗并保存到es和opentsdb
6. 通过flink job进行服务依赖分析报错快速归类分析
7. 基于springmvc实现es和opentsdb数据查询的rest api查询接口

成就:  
为公司200+服务, 1000多台机器提供服务

#### 链路追踪系统
项目介绍: 基于日志rpcid和reqid的第一代链路追踪系统  
技术栈: java, springmvc, spark, hbase  
主要工作:  
1. 配置logagent收集soa日志，发送到kafka
2. 通过spark任务消费kafa进行数据清洗并保存到hbase
3. 通过spark任务进行服务依赖分析
4. 通过spark进行soa服务tps和错误数统计
5. 开发基于springmvc的数据查询web服务

#### 分布式配置中心
项目介绍: 基于开源方案apoll二次开发的分布式配置中心
技术栈: java, springmvc, pg
主要工作: 
1. 数据库适配pg工作
2. 接入公司单点登录功能
3. 开发配置发布邮件通知功能
4. 客户端自动获取service功能
5. 服务部署接入发布系统适配工作s

#### 骑行轨迹系统
项目介绍: 提供骑行轨迹收集和查询功能的系统  
技术栈: java, kafka, hbase  
主要工作: 
1. rowkey设计, 订单号+时间逆序
2. 消费kafka数据并将骑行轨迹写入hbase
3. 通过scan实现骑行轨迹查询接口

#### 海量数据收集系统
项目介绍: 哈啰海量数据收集系统，日收集数据30TB，系统包含：服务器本地日志收集器LogAgent，插件式服务数据收集器            yukonLogger
技术栈: java, disruptor, kafka
主要工作: 
1. 使用disruptor队列实现低延迟日志收集器
2. 实现日志断点续传功能
3. 通过通配符实现新日志自动识别并收集
4. 实现云端日志收集器配置功能
5. 实现日志翻滚和识别处理功能
6. 编写支持上报kafak或写本地日志的埋点组件sdk

#### tcp服务器
项目介绍: 实现一个基于netty的tcp服务器，实现移动端和服务器的双向通信  
技术栈: java, netty  
主要工作:  
1. 设计自定义协议栈
2. 基于netty开发服务器
3. 提供基于netty开发的android客户端sdk

### 上海携程网络技术有限公司    C++开发工程师                   2014.07 - 2015.08
#### 软交换客服呼叫中心
项目介绍: 搭建一个基于软件交换的客服呼叫中心  
技术栈: c++, freeswitch  
主要工作:  
1. 呼叫中心客户端适配开发
2. 编写呼叫路由规则脚本
3. 协助呼叫中心服务器压测工作  

第一年软交换客服呼叫中心迁入1000+客户, 每年节省硬件成本上百万

## 教育背景
上海，同济大学，本科  2010.09 - 2014.07

## 技能
技术栈  
1. java, spring, redis, mybatis, mysql, pg, redis
2. kafak, spark, flink
3. es, clickhouse, hbase
4. git, maven