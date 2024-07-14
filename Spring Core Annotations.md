
### 1. @Autowired

We can use the _@Autowired_ to **mark a dependency which Spring is going to resolve and inject**. We can use this annotation with a constructor, setter, or field injection.

Constructor injection:
```java
class Car {
    Engine engine;

    @Autowired
    Car(Engine engine) {
        this.engine = engine;
    }
}
```

Setter injection:
```java
class Car {
    Engine engine;

    @Autowired
    void setEngine(Engine engine) {
        this.engine = engine;
    }
}
```

Field injection:
```java
class Car {
    @Autowired
    Engine engine;
}
```

_@Autowired_ has a _boolean_ argument called _required_ with a default value of _true_. It tunes Spring’s behavior when it doesn’t find a suitable bean to wire. When _true_, an exception is thrown, otherwise, nothing is wired.

### 2. @Bean

_@Bean_ marks a factory method which instantiates a Spring bean:

```java
@Bean
Engine engine() {
    return new Engine();
}
```

**Spring calls these methods** when a new instance of the return type is required. Note, that all methods annotated with _@Bean_ must be in _@Configuration_ classes.

### 3. @Qualifier

We use _@Qualifier_ along with _@Autowired_ to **provide the bean id or bean name** we want to use in ambiguous situations.

For example, the following two beans implement the same interface:

```java
class Bike implements Vehicle {}

class Car implements Vehicle {}
```

If Spring needs to inject a _Vehicle_ bean, it ends up with multiple matching definitions. In such cases, we can provide a bean’s name explicitly using the _@Qualifier_ annotation.

```java
@Autowired
@Qualifier("bike")
Vehicle vehicle;
```

### 4. @Value

We can use _@Value_ for injecting property values into beans. It’s compatible with constructor, setter, and field injection. We can use **placeholder strings** in _@Value_ to wire values **defined in external sources**, for example, in _.properties_ or _.yaml_ files.

Let’s assume the following _.properties_ file:

```plaintext
engine.fuelType=petrol
```

We can inject the value of _engine.fuelType_ with the following:

```java
@Value("${engine.fuelType}")
String fuelType;
```

### 5. @DependsOn

We can use this annotation to make [[Spring Framework]] **initialize other beans before the annotated one**. Usually, this behavior is automatic, based on the explicit dependencies between beans.

We only need this annotation **when the dependencies are implicit**, for example, JDBC driver loading or static variable initialization.

We can use _@DependsOn_ on the dependent class specifying the names of the dependency beans. The annotation’s _value_ argument needs an array containing the dependency bean names:

```java
@DependsOn("engine")
class Car implements Vehicle {}
```

Alternatively, if we define a bean with the _@Bean_ annotation, the factory method should be annotated with _@DependsOn_:

```java
@Bean
@DependsOn("fuel")
Engine engine() {
    return new Engine();
}
```

### 6. @Lazy

We use _@Lazy_ when we want to initialize our bean lazily. By default, Spring creates all singleton beans eagerly at the startup/bootstrapping of the application context.

However, there are cases when **we need to create a bean when we request it, not at application startup**.

This annotation behaves differently depending on where we exactly place it. We can put it on:

- a _@Bean_ annotated bean factory method, to delay the method call (hence the bean creation)
- a @_Configuration_ class and all contained _@Bean_ methods will be affected
- a _@Component_ class, which is not a _@Configuration_ class, this bean will be initialized lazily
- an _@Autowired_ constructor, setter, or field, to load the dependency itself lazily (via proxy)

### 7. @Primary

Sometimes we need to define multiple beans of the same type. In these cases, the injection will be unsuccessful because Spring has no clue which bean we need.

We already saw an option to deal with this scenario: marking all the wiring points with _@Qualifier_ and specify the name of the required bean.

However, most of the time we need a specific bean and rarely the others. We can use _@Primary_ to simplify this case: if we mark the most frequently used bean with _@Primary_ it will be chosen on unqualified injection points:

```java
@Component
@Primary
class Car implements Vehicle {}

@Component
class Bike implements Vehicle {}

@Component
class Driver {
    @Autowired
    Vehicle vehicle;
}

@Component
class Biker {
    @Autowired
    @Qualifier("bike")
    Vehicle vehicle;
}
```

### 8. @Profile

If we want Spring to use a _@Component_ class or a _@Bean_ method only when a specific profile is active, we can mark it with _@Profile_. We can configure the name of the profile with the _value_ argument of the annotation:

```java
@Component
@Profile("sportDay")
class Bike implements Vehicle {}
```

### 9. @Import

We can use specific _@Configuration_ classes without component scanning with this annotation. We can provide those classes with _@Import_ value argument:

```java
@Import(VehiclePartSupplier.class)
class VehicleFactoryConfig {}
```

### 10. @ImportResource

We can **import XML configurations** with this annotation. We can specify the XML file locations with the _locations_ argument, or with its alias, the _value_ argument:

```java
@Configuration
@ImportResource("classpath:/annotations.xml")
class VehicleFactoryConfig {}
```

### 3.4. _@PropertySource_[](https://www.baeldung.com/spring-core-annotations#property-source)

With this annotation, we can **define property files for application settings**:
```java
@Configuration
@PropertySource("classpath:/annotations.properties")
class VehicleFactoryConfig {}
```

_@PropertySource_ leverages the Java 8 repeating annotations feature, which means we can mark a class with it multiple times:

```java
@Configuration
@PropertySource("classpath:/annotations.properties")
@PropertySource("classpath:/vehicle-factory.properties")
class VehicleFactoryConfig {}
```