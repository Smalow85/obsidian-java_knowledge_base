A _join point_ is a step of the program execution, such as the execution of a method or the handling of an exception. In [[Spring AOP]], a join point always represents a method execution. A _pointcut_ is a predicate that matches the join points, and the _pointcut expression language_ is a way of describing pointcuts programmatically.

A pointcut expression can appear as a value of the _@Pointcut_ annotation:

```java
@Pointcut("within(@org.springframework.stereotype.Repository *)")
public void repositoryClassMethods() {}
```

The method declaration is called the **pointcut signature**. It provides a name that advice annotations can use to refer to that pointcut:

```java
@Around("repositoryClassMethods()")
public Object measureMethodExecutionTime(ProceedingJoinPoint pjp) throws Throwable {
    ...
}
```

## Pointcut Designators

A pointcut expression starts with a **pointcut designator (PCD)**, which is a keyword telling Spring AOP what to match. There are several pointcut designators, such as the execution of a method, type, method arguments, or annotations.

### 1. execution

The primary Spring PCD is _execution_, which matches method execution join points:

```java
@Pointcut("execution(public String com.baeldung.pointcutadvice.dao.FooDao.findById(Long))")
```

This example pointcut will exactly match the execution of the _findById_ method of the _FooDao_ class. This works, but it’s not very flexible. Suppose we’d like to match all the methods of the _FooDao_ class, which may have different signatures, return types, and arguments. To achieve this, we can use wildcards:

```java
@Pointcut("execution(* com.baeldung.pointcutadvice.dao.FooDao.*(..))")
```

Here, the first wildcard matches any return value, the second matches any method name, and the _(..)_ pattern matches any number of parameters (zero or more).

### 2. within

Another way to achieve the same result as the previous section is by using the _within_ PCD, which limits matching to join points of certain types:

```java
@Pointcut("within(com.baeldung.pointcutadvice.dao.FooDao)")
```

We can also match any type within the _com.baeldung_ package or a sub-package:

```java
@Pointcut("within(com.baeldung..*)")
```

### 3. this and target

_this_ limits matching to join points where the bean reference is an instance of the given type, while _target_ limits matching to join points where the target object is an instance of the given type. The former works when Spring AOP creates a CGLIB-based proxy, and the latter is used when a JDK-based proxy is created. Suppose that the target class implements an interface:

```java
public class FooDao implements BarDao {
    ...
}
```

In this case, Spring AOP will use the JDK-based proxy, and we should use the _target_ PCD because the proxied object will be an instance of the _Proxy_ class and implement the _BarDao_ interface:

```java
@Pointcut("target(com.baeldung.pointcutadvice.dao.BarDao)")
```

On the other hand, if _FooDao_ doesn’t implement any interface, or the _proxyTargetClass_ property is set to true, then the proxied object will be a subclass of _FooDao_ and we can use the _this_ PCD:

```java
@Pointcut("this(com.baeldung.pointcutadvice.dao.FooDao)")
```

### 4. args

```java
@Pointcut("execution(* *..find*(Long))")
```

This pointcut matches any method that starts with find and has only one parameter of type _Long_. If we want to match a method with any number of parameters, but still having the fist parameter of type _Long_, we can use the following expression:

```java
@Pointcut("execution(* *..find*(Long,..))")
```

### 5. @target

The _@target_ PCD (not to be confused with the _target_ PCD described above) limits matching to join points where the class of the executing object has an annotation of the given type:

```java
@Pointcut("@target(org.springframework.stereotype.Repository)")
```

### 6. @args

This PCD limits matching to join points where the runtime type of the actual arguments passed have annotations of the given type(s). Suppose that we want to trace all the methods accepting beans annotated with the _@Entity_ annotation:

```java
@Pointcut("@args(com.baeldung.pointcutadvice.annotations.Entity)")
public void methodsAcceptingEntities() {}
```

To access the argument, we should provide a _JoinPoint_ argument to the advice:

```java
@Before("methodsAcceptingEntities()")
public void logMethodAcceptionEntityAnnotatedBean(JoinPoint jp) {
    logger.info("Accepting beans with @Entity annotation: " + jp.getArgs()[0]);
}
```

### 7. @within

This PCD limits matching to join points within types that have the given annotation:

```java
@Pointcut("@within(org.springframework.stereotype.Repository)")
```

Which is equivalent to:

```java
@Pointcut("within(@org.springframework.stereotype.Repository *)")
```

### 8. @annotation

This PCD limits matching to join points where the subject of the join point has the given annotation. For example, we can create a _@Loggable_ annotation:

```java
@Pointcut("@annotation(com.baeldung.pointcutadvice.annotations.Loggable)")
public void loggableMethods() {}
```

Then we can log the execution of the methods marked by that annotation:

```java
@Before("loggableMethods()")
public void logMethod(JoinPoint jp) {
    String methodName = jp.getSignature().getName();
    logger.info("Executing method: " + methodName);
}
```