---
title: "Spring Aop"
summary: "介绍spring aop的概念和实战例子"
keywords: "spring,aop"

date: 2026-02-08T23:24:32+08:00
lastmod: 2026-02-08T23:24:32+08:00

math: false
mermaid: false

categories:
  - spring
tags:
  - aop
---

## 定义一个Aspect

```JAVA
@Aspect
public class NotVeryUsefulAspect {
}
```

## 定义一个Pointcut

```JAVA
@Pointcut("execution(* transfer(..))") // the pointcut expression
private void anyOldTransfer() {} // the pointcut signature
```

### 切入点表达式

1. execute: 拦截方法
2. within: 拦截类名
3. this: 代理对象为指定的类型会被拦截
4. target: 目标对象为指定的类型被拦截
5. args: 匹配方法中的参数
6. @target: 匹配的目标对象的类有一个指定的注解
7. @within: 指定匹配必须包含某个注解的类里的所有连接点
8. @annotation: 匹配有指定注解的方法（注解作用在方法上面）
9. @args: 方法参数所属的类型上有指定的注解，被匹配

## 定义一个Advice

### Before Advice

```JAVA
@Aspect
public class BeforeExample {

  @Before("execution(* com.xyz.dao.*.*(..))")
  public void doAccessCheck() {
    // ...
  }
}
```

### After Returning Advice

```JAVA
@Aspect
public class AfterReturningExample {

  @AfterReturning("execution(* com.xyz.dao.*.*(..))")
  public void doAccessCheck() {
    // ...
  }
}
```

### After Throwing Advice

```JAVA
@Aspect
public class AfterThrowingExample {

  @AfterThrowing("execution(* com.xyz.dao.*.*(..))")
  public void doRecoveryActions() {
    // ...
  }
}
```

### After (Finally) Advice

```JAVA
@Aspect
public class AfterFinallyExample {

  @After("execution(* com.xyz.dao.*.*(..))")
  public void doReleaseLock() {
    // ...
  }
}
```

### Around Advice

```JAVA
@Aspect
public class AroundExample {

  @Around("execution(* com.xyz..service.*.*(..))")
  public Object doBasicProfiling(ProceedingJoinPoint pjp) throws Throwable {
    // start stopwatch
    Object retVal = pjp.proceed();
    // stop stopwatch
    return retVal;
  }
}
```

## 完整例子

```JAVA
@Component
@Aspect
public class AopAspect {

    // 定义一个空方法，借用其注解抽取切点表达式
    @Pointcut("@annotation(com.example.demo.aop.AopAnnotation)")
    public void pointCut() {
    }

    // 环绕通知
    @Around("pointCut()")
    public Object around(ProceedingJoinPoint pjp) {
        System.out.println("----------------环绕通知之前 的部分(相当于前置通知)----------------");
        // 获取到类名
        String targetName = pjp.getTarget().getClass().getName();
        System.out.println("代理的类是:" + targetName);
        // 获取到参数
        Object[] parameter = pjp.getArgs();
        System.out.println("传入的参数是:" + Arrays.toString(parameter));
        // 获取到方法签名，进而获得方法
        MethodSignature signature = (MethodSignature) pjp.getSignature();
        Method method = signature.getMethod();
        System.out.println("增强的方法名字是:" + method.getName());

        // 处理一些业务逻辑
        // 模拟简单的获取参数
        AopAnnotation annotation = method.getAnnotation(AopAnnotation.class);
        String value = annotation.value();
        System.out.println("注解参数值: value\t" + value);
        // spel 解析参数
        Parameter[] parameters = method.getParameters();
        String idSpel = annotation.idSpel();
        if (ArrayUtil.isNotEmpty(parameters)) {
            ExpressionParser parser = new SpelExpressionParser();
            EvaluationContext context = new StandardEvaluationContext();
            for (int index = 0, length_1 = parameters.length; index &lt; length_1; index++) {
                String paramterName = parameters[index].getName();
                Object paramterValue = parameter[index];
                context.setVariable(paramterName, paramterValue);
            }
            String value1 = parser.parseExpression(idSpel).getValue(context, String.class);
            System.out.println(String.format("idSpel: %s, value: %s", idSpel, value1));
        } else {
            System.out.println("idSpel 参数获取不到上下文环境, idSpel: " + idSpel);
        }

        // 让方法执行
        Object proceed = null;
        try {
            proceed = pjp.proceed();
            // 环绕通知之后的业务逻辑部分
            System.out.println("----------------环绕通知之后的部分(相当于后置通知AfterReturning)-----------------");
        } catch (Throwable e) {
            System.out.println("-------------环绕通知的异常部分(相当于异常通知AfterThrowing)--------------------------");
            e.printStackTrace();
        } finally {
            System.out.println("-------------环绕通知的最终部分部分(相当于最终通知After)--------------------------");
        }

        return proceed;
    }

}
```
