Spring `@Bean` annotation is applied on a method to specify that it returns a bean to be managed by Spring context.

```java
@Bean
Engine engine() {
    return new Engine();
}
```

**Spring calls these methods** when a new instance of the return type is required.

The resulting bean has the same name as the factory method. If we want to name it differently, we can do so with the _name_ or the _value_ arguments of this annotation (the argument _value_ is an alias for the argument _name_):

```java
@Bean("engine")
Engine getEngine() {
    return new Engine();
}
```

Note, that all methods annotated with @Bean must be in [[@Configuration]] classes.