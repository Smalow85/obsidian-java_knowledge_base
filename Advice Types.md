**Advice** is an action taken by an aspect at a particular join point. **Different types of advice include “around,” “before” and “after” advice.** The main purpose of aspects is to support cross-cutting concerns, such as logging, profiling, caching, and transaction management.

To enable advice in AspectJ, you must first apply the _@EnableAspectJAutoProxy_ annotation to your configuration class, which will enable support for handling components marked with AspectJ’s _@Aspect_ annotation. In Spring Boot projects, we don’t have to explicitly use the _@EnableAspectJAutoProxy_. There’s a dedicated [_AopAutoConfiguration_](https://docs.spring.io/spring-boot/docs/current/api/org/springframework/boot/autoconfigure/aop/AopAutoConfiguration.html) that enables [[Spring AOP]] support if the _Aspect_ or _Advice_ is on the classpath.

## Before Advice

**This advice, as the name implies, is executed before the join point.** It doesn’t prevent the continued execution of the method it advises unless an exception is thrown.

```java
@Component
@Aspect
public class LoggingAspect {

    private Logger logger = Logger.getLogger(LoggingAspect.class.getName());

    @Pointcut("@target(org.springframework.stereotype.Repository)")
    public void repositoryMethods() {};

    @Before("repositoryMethods()")
    public void logMethodCall(JoinPoint jp) {
        String methodName = jp.getSignature().getName();
        logger.info("Before " + methodName);
    }
}
```

## After Advice

After advice, declared by using the _@After_ annotation, is executed after a matched method’s execution, whether or not an exception was thrown.
In some ways, it is similar to a _finally_ block. In case you need advice to be triggered only after normal execution, you should use the _returning advice_ declared by _@AfterReturning_ annotation. If you want your advice to be triggered only when the target method throws an exception, you should use _throwing advice,_ declared by using the _@AfterThrowing_ annotation.

```java
@Component
@Aspect
public class PublishingAspect {

    private ApplicationEventPublisher eventPublisher;

    @Autowired
    public void setEventPublisher(ApplicationEventPublisher eventPublisher) {
        this.eventPublisher = eventPublisher;
    }

    @Pointcut("@target(org.springframework.stereotype.Repository)")
    public void repositoryMethods() {}

    @Pointcut("execution(* *..create*(Long,..))")
    public void firstLongParamMethods() {}

    @Pointcut("repositoryMethods() && firstLongParamMethods()")
    public void entityCreationMethods() {}

    @AfterReturning(value = "entityCreationMethods()", returning = "entity")
    public void logMethodCall(JoinPoint jp, Object entity) throws Throwable {
        eventPublisher.publishEvent(new FooCreationEvent(entity));
    }
}
```

## Around Advice

**Around advice** surrounds a join point such as a method invocation. This is the most powerful kind of advice. **Around advice can perform custom behavior both before and after the method invocation.** It’s also responsible for choosing whether to proceed to the join point or to shortcut the advised method execution by providing its own return value or throwing an exception.
```java
@Aspect
@Component
public class PerformanceAspect {

    private Logger logger = Logger.getLogger(getClass().getName());

    @Pointcut("within(@org.springframework.stereotype.Repository *)")
    public void repositoryClassMethods() {};

    @Around("repositoryClassMethods()")
    public Object measureMethodExecutionTime(ProceedingJoinPoint pjp) throws Throwable {
        long start = System.nanoTime();
        Object retval = pjp.proceed();
        long end = System.nanoTime();
        String methodName = pjp.getSignature().getName();
        logger.info("Execution of " + methodName + " took " + 
          TimeUnit.NANOSECONDS.toMillis(end - start) + " ms");
        return retval;
    }
}
```

This advice is triggered when any of the join points matched by the _repositoryClassMethods_ pointcut is executed.
This advice takes one parameter of type _ProceedingJointPoint_. The parameter gives us an opportunity to take action before the target method call. **In this case, we simply save the method start time.

Second, the advice return type is _Object_ since the target method can return a result of any type. If target method is _void,_ _null_ will be returned. After the target method call, we can measure the timing, log it, and return the method’s result value to the caller.


