A circular dependency in [[Spring Framework]] occurs when a bean A depends on another bean B, and the bean B depends on bean A as well:

Bean A → Bean B → Bean A
## What Happens in Spring

When the Spring context loads all the beans, it tries to create beans in the order needed for them to work completely.

Let’s say we don’t have a circular dependency. We instead have something like this:

**Bean A → Bean B → Bean C**

Spring will create bean C, then create bean B (and inject bean C into it), then create bean A (and inject bean B into it).

But with a circular dependency, Spring cannot decide which of the beans should be created first since they depend on one another. In these cases, Spring will raise a _BeanCurrentlyInCreationException_ while loading context.
## Workarounds

### 1. Redesign
When we have a circular dependency, it’s likely we have a design problem and that the responsibilities are not well separated. We should try to redesign the components properly so that their hierarchy is well designed and there is no need for circular dependencies.
### 2. Use @Lazy
A simple way to break the cycle is by telling Spring to initialize one of the beans lazily. So, instead of fully initializing the bean, it will create a proxy to inject it into the other bean. The injected bean will only be fully created when it’s first needed.

### 3. Use Setter/Field Injection
One of the most popular workarounds, and also what the [Spring documentation suggests](http://docs.spring.io/spring/docs/current/spring-framework-reference/html/beans.html), is using setter injection. Simply put, we can address the problem by changing the way our beans are wired – to use setter injection (or field injection) instead of constructor injection. This way, Spring creates the beans, but the dependencies aren’t injected until they are needed.

### 4. @PostConstruct
Another way to break the cycle is by injecting a dependency using _@Autowired_ on one of the beans and then using a method annotated with _@PostConstruct_ to set the other dependency.

```java
@Component
public class CircularDependencyA {

    @Autowired
    private CircularDependencyB circB;

    @PostConstruct
    public void init() {
        circB.setCircA(this);
    }

    public CircularDependencyB getCircB() {
        return circB;
    }
}
```

```java
@Component
public class CircularDependencyB {

    private CircularDependencyA circA;
	
    private String message = "Hi!";

    public void setCircA(CircularDependencyA circA) {
        this.circA = circA;
    }
	
    public String getMessage() {
        return message;
    }
}
```

