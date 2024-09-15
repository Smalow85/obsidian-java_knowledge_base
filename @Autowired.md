To use Java-based configuration in our application, let’s enable annotation-driven injection to load our Spring configuration:

```java
@Configuration
@ComponentScan("com.baeldung.autowire.sample")
public class AppConfig {}
```

Moreover, Spring Boot introduces the [[@SpringBootApplication]] annotation. This single annotation is equivalent to using [[@Configuration]], [[@EnableAutoConfiguration]], and [[@ComponentScan]].

After enabling annotation injection, **we can use autowiring on properties, setters, and constructors**.

```java
@Component
public class FooService {  
    @Autowired
    private FooFormatter fooFormatter;
}
```

```java
public class FooService {
    private FooFormatter fooFormatter;
    @Autowired
    public void setFormatter(FooFormatter fooFormatter) {
        this.fooFormatter = fooFormatter;
    }
}
```

```java
public class FooService {
    private FooFormatter fooFormatter;
    @Autowired
    public void setFormatter(FooFormatter fooFormatter) {
        this.fooFormatter = fooFormatter;
    }
}
```

By default, Spring resolves `@Autowired` entries by type. If more than one bean of the same type is available in the container, the framework will throw a fatal exception.

To fix this issue, we can use the following options:

1. [[@Qualifier]]

```java
public class FooService {
    @Autowired
    @Qualifier("fooFormatter")
    private Formatter formatter;
}
```

2. Custom Qualifier

Spring also allows us to create our own custom `@Qualifier` annotation. To do so, we should provide the `@Qualifier` annotation with the definition:

```java
@Qualifier
@Target({
  ElementType.FIELD, ElementType.METHOD, ElementType.TYPE, ElementType.PARAMETER})
@Retention(RetentionPolicy.RUNTIME)
public @interface FormatterType {  
    String value();
}
```

```java
@FormatterType("Foo")
@Component
public class FooFormatter implements Formatter {
    public String format() {
        return "foo";
    }
}
```

3. Autowiring by Name

Spring uses the bean’s name as a default qualifier value. It will inspect the container and look for a bean with the exact name as the property to autowire it.

Hence, in our example, Spring matches the `fooFormatter` property name to the `FooFormatter` implementation. Therefore, it injects that specific implementation when constructing `FooService`:

```java
public class FooService {
 @Autowired 
private Formatter fooFormatter; 
}
```

