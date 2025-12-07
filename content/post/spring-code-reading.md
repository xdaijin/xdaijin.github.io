---
title: "Spring框架解析"
summary: "这篇文章介绍了spring框架的使用内容"
keywords: "spring"

date: 2025-12-07T22:56:38+08:00
lastmod: 2025-12-07T22:56:38+08:00

math: false
mermaid: false

categories:
  - java
tags:
  - spring
---

## spring

### aop

## spring boot

### sping类的两种主要加载机制

1. spring bean
    - 注解
  
        ```JAVA
        @Service
        @Component
        @Bean
        ...
        ```
  
    - applicationContext.xml
  
        ```XML
        <?xml version="1.0" encoding="UTF-8"?>
        <beans xmlns="http://www.springframework.org/schema/beans"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">
          <bean id="test" class="com.example.dao.TestDaoImpl"/>
        </beans>
        ```

2. SpringFactoriesLoader
    在resources目录下加上META-INF/spring.factories：

    ```properties
    org.springframework.context.ApplicationListener=kyoo.order.listener.MyApplicationListener
    ```

    主要支持类：

    - BootstrapRegistryInitializer
    - ApplicationContextInitializer
    - ApplicationListener
    - SpringApplicationRunListener

### spring boot应用启动过程

#### 主要阶段

SpringApplicationRunListener类明确标出了spring应用启动的过程：

1. starting
2. environmentPrepared
3. contextPrepared
4. contextLoaded
5. started
6. ready

还有一个异常阶段：

1. failed

#### 核心代码

org.springframework.boot.SpringApplication类的run方法：

```JAVA
 public ConfigurableApplicationContext run(String... args) {
        Startup startup = SpringApplication.Startup.create();
        if (this.properties.isRegisterShutdownHook()) {
            shutdownHook.enableShutdownHookAddition();
        }

        DefaultBootstrapContext bootstrapContext = this.createBootstrapContext();
        ConfigurableApplicationContext context = null;
        this.configureHeadlessProperty();
        SpringApplicationRunListeners listeners = this.getRunListeners(args);
        listeners.starting(bootstrapContext, this.mainApplicationClass);

        try {
            ApplicationArguments applicationArguments = new DefaultApplicationArguments(args);
            ConfigurableEnvironment environment = this.prepareEnvironment(listeners, bootstrapContext, applicationArguments);
            Banner printedBanner = this.printBanner(environment);
            context = this.createApplicationContext();
            context.setApplicationStartup(this.applicationStartup);
            this.prepareContext(bootstrapContext, context, environment, listeners, applicationArguments, printedBanner);
            this.refreshContext(context);
            this.afterRefresh(context, applicationArguments);
            Duration timeTakenToStarted = startup.started();
            if (this.properties.isLogStartupInfo()) {
                (new StartupInfoLogger(this.mainApplicationClass, environment)).logStarted(this.getApplicationLog(), startup);
            }

            listeners.started(context, timeTakenToStarted);
            this.callRunners(context, applicationArguments);
        } catch (Throwable ex) {
            throw this.handleRunFailure(context, ex, listeners);
        }

        try {
            if (context.isRunning()) {
                listeners.ready(context, startup.ready());
            }

            return context;
        } catch (Throwable ex) {
            throw this.handleRunFailure(context, ex, (SpringApplicationRunListeners)null);
        }
    }
```

##### 构造SpringApplication

通过SpringFactoriesLoader加载下面初始化器：

- BootstrapRegistryInitializer 用来初始化启动上下文
- ApplicationContextInitializer 应用上下文初始化器
- ApplicationListener 事件监听器

##### 初始化启动上下文

通过BootstrapRegistryInitializer来初始化启动上下文

触发事件starting

##### 解析应用配置

通过prepareEnvironment方法解析应用配置

触发事件environmentPrepared

##### 创建并初始化上下文

创建上下文并调用ApplicationContextInitializer来初始化

触发事件contextPrepared和contextLoaded

##### 刷新上下文

调用AbstractApplicationContext的refresh方法刷新上下文

触发事件contextRefreshed和started

##### 最后阶段

同步执行所有runner

触发事件ready

### bean生命周期

## spring web

### web三个组件

- Servlet
- Filter
- EventListener

注册方式：

- filter

    ```JAVA
    @Component
    public class MyFilter implements Filter {}
    或
    @Bean
    public FilterRegistrationBean<Filter> myFilterRegistration() {
        FilterRegistrationBean<Filter> filterRegistrationBean = new FilterRegistrationBean<>(new MyAnotherFilter());
        return filterRegistrationBean;
    }
    或
    @ServletComponentScan("kyoo.order.filter")
    public class MyWebConfig implements WebMvcConfigurer {}
    ...
    @WebFilter
    public class MyThirdFilter implements Filter {}
    ```

- listener

    ```JAVA
    @Component
    public class MyServletContextListener implements ServletContextListener {}
    或
    @Bean
    public ServletListenerRegistrationBean<ServletContextListener> myServletListenerRegistration() {
        ServletListenerRegistrationBean<ServletContextListener> registrationBean = new ServletListenerRegistrationBean<>(new MySecondServletContextListener());
        return registrationBean;
    }
    或
    @ServletComponentScan("kyoo.order.listener")
    public class MyWebConfig implements WebMvcConfigurer {}
    ...
    @WebListener
    public class MyThirdServletListener implements ServletContextListener {}
    ```

### 核心类DispatcherServlet

实际上是一个HttpServlet，绑定了路径`/`，由DispatcherServletAutoConfiguration类配置

核心代码：

```JAVA
protected void doDispatch(HttpServletRequest request, HttpServletResponse response) throws Exception {
  HttpServletRequest processedRequest = request;
  HandlerExecutionChain mappedHandler = null;
  boolean multipartRequestParsed = false;

  WebAsyncManager asyncManager = WebAsyncUtils.getAsyncManager(request);

  try {
    ModelAndView mv = null;
    Exception dispatchException = null;

    try {
      processedRequest = checkMultipart(request);
      multipartRequestParsed = (processedRequest != request);

      // Determine handler for the current request.
      mappedHandler = getHandler(processedRequest);
      if (mappedHandler == null) {
        noHandlerFound(processedRequest, response);
        return;
      }

      if (!mappedHandler.applyPreHandle(processedRequest, response)) {
        return;
      }

      // Determine handler adapter and invoke the handler.
      HandlerAdapter ha = getHandlerAdapter(mappedHandler.getHandler());
      mv = ha.handle(processedRequest, response, mappedHandler.getHandler());

      if (asyncManager.isConcurrentHandlingStarted()) {
        return;
      }

      applyDefaultViewName(processedRequest, mv);
      mappedHandler.applyPostHandle(processedRequest, response, mv);
    }
    catch (Exception ex) {
      dispatchException = ex;
    }
    catch (Throwable err) {
      // As of 4.3, we're processing Errors thrown from handler methods as well,
      // making them available for @ExceptionHandler methods and other scenarios.
      dispatchException = new ServletException("Handler dispatch failed: " + err, err);
    }
    processDispatchResult(processedRequest, response, mappedHandler, mv, dispatchException);
  }
  catch (Exception ex) {
    triggerAfterCompletion(processedRequest, response, mappedHandler, ex);
  }
  catch (Throwable err) {
    triggerAfterCompletion(processedRequest, response, mappedHandler,
        new ServletException("Handler processing failed: " + err, err));
  }
  finally {
    if (asyncManager.isConcurrentHandlingStarted()) {
      // Instead of postHandle and afterCompletion
      if (mappedHandler != null) {
        mappedHandler.applyAfterConcurrentHandlingStarted(processedRequest, response);
      }
      asyncManager.setMultipartRequestParsed(multipartRequestParsed);
    }
    else {
      // Clean up any resources used by a multipart request.
      if (multipartRequestParsed || asyncManager.isMultipartRequestParsed()) {
        cleanupMultipart(processedRequest);
      }
    }
  }
}
```
