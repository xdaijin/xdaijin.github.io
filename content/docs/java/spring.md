# Spring

## Spring核心技术
### IoC容器
通过IoC容器技术对象们只需要申明需要的依赖，而不需要自己去构造或者通过其他机制其寻找依赖，IoC容器会在对象创建之后把他需要的依赖注入进来

依赖注入(DI)是实现IoC的一种方式，支持construct-based和setter-based方式，推荐construct-based

ApplicationContext是Spring的IoC容器

### 数据校验，绑定，类型转换
没研究透，没怎么用过

### Resources
强化对资源的控制和处理

### AOP
面向切面编程和和面向对象编程不一样的另一种编程结构，切面的思想就是把一段代码或行为重复作用到不同的对象上去，典型使用场景有事务管理
、权限校验和日志打印等

核心概念：
* Aspect: 切面, 就是一段通用的行为
* Join Point: 连结的时间点，比如方法执行的时候，异常抛出的时候
* Advice: 通知的时间点，就是在join Point的前后

### Null-safety
对null值的声明的注解

### Data Buffers and Codecs
统一buffer类

### Logging
日志统一功能

### Ahead of Time Optimizations
AOT没用过

### Spring boot启动流程

## SpringBoot
### SpringApplication.run
1.开启计时
2.获取监听器
3.配置environment
    用来选择dev、test、pro环境的profile
4.prepareContext
    设置一些beanFactory之类的
5.refreshContext
    主要就是创建bean
6.afterRefresh
    refresh之后的一些逻辑，默认是空
7.callRunners

### Bean生命周期
* 实例化
* 属性赋值
* 初始化
* 销毁
  
## SpringMVC
### 核心概念
* DispatcherServlet
* HandlerMapping
* HandlerAdapter
* Handler
* ModelMap
* ModelAndView
* ViewResolver
* View

### DispatcherServlet流程
1.根据url获取调用HandlerMapping获取handler
2.调用handler处理请求
3.handler处理完返回ModleAndView
4.调用ViewResolver解析后返回具体View
5.Dispatcher调用view的render方法进行渲染