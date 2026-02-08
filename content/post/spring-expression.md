---
title: "Spring Expression"
summary: "srping expression使用例子"
keywords: "spring,expression"

date: 2026-02-08T23:54:23+08:00
lastmod: 2026-02-08T23:54:23+08:00

math: false
mermaid: false

categories:
  - spring
tags:
  - spel
---

## 结合AOP使用spel

```JAVA
publicclass SpelUtil {

    /**
     * 用于SpEL表达式解析.
     */
    private static final SpelExpressionParser parser = new SpelExpressionParser();

    /**
     * 用于获取方法参数定义名字.
     */
    private static final DefaultParameterNameDiscoverer nameDiscoverer = new DefaultParameterNameDiscoverer();

    /**
     * 解析SpEL表达式
     *
     * @param spELStr
     * @param joinPoint
     * @return
     */
    public static String generateKeyBySpEL(String spELStr, ProceedingJoinPoint joinPoint) {
        // 通过joinPoint获取被注解方法
        MethodSignature methodSignature = (MethodSignature) joinPoint.getSignature();
        Method method = methodSignature.getMethod();
        // 使用Spring的DefaultParameterNameDiscoverer获取方法形参名数组
        String[] paramNames = nameDiscoverer.getParameterNames(method);
        // 解析过后的Spring表达式对象
        Expression expression = parser.parseExpression(spELStr);
        // Spring的表达式上下文对象
        EvaluationContext context = new StandardEvaluationContext();
        // 通过joinPoint获取被注解方法的形参
        Object[] args = joinPoint.getArgs();
        // 给上下文赋值
        for (int i = 0; i < args.length; i++) {
            context.setVariable(paramNames[i], args[i]);
        }
        // 表达式从上下文中计算出实际参数值
        /*如:
            @annotation(key="#user.name")
            method(User user)
             那么就可以解析出方法形参的某属性值，return “xiaoming”;
          */
        return expression.getValue(context).toString();
    }
}


@Aspect
@Slf4j
@Component
public class SpelGetParmAop {

    @PostConstruct
    public void init() {
        log.info("SpelGetParm init ......");
    }
    /**
     * 拦截加了SpelGetParm注解的方法请求
     *
     * @param joinPoint
     * @param spelGetParm
     * @return
     * @throws Throwable
     */
    @Around("@annotation(spelGetParm)")
    public Object beforeInvoce(ProceedingJoinPoint joinPoint, SpelGetParm spelGetParm) throws Throwable {
        Object result = null;
        // 方法名
        String methodName = joinPoint.getSignature().getName();
        //获取动态参数
        String parm = SpelUtil.generateKeyBySpEL(spelGetParm.parm(), joinPoint);
        log.info("spel获取动态aop参数: {}", parm);
        try {
            log.info("执行目标方法: {} ==>>开始......", methodName);
            result = joinPoint.proceed();
            log.info("执行目标方法: {} ==>>结束......", methodName);
            // 返回通知
            log.info("目标方法 " + methodName + " 执行结果 " + result);
        } finally {

        }
        // 后置通知
        log.info("目标方法 " + methodName + " 结束");
        return result;
    }
}
```