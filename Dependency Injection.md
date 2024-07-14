Dependency Injection is **a fundamental aspect of the [[Spring Framework]]**, through which the [[Spring Container]] “injects” objects into other objects or “dependencies”.

Simply put, this allows for **loose coupling** of components and **moves the responsibility of managing components onto the container**.

**Dependency Injection in Spring can be done through constructors, setters or fields.**
### Constructor-Based Dependency Injection
In the case of constructor-based dependency injection, the container will invoke a constructor with arguments each representing a dependency we want to set.

```java
@Configuration
@ComponentScan("com.baeldung.constructordi")
public class Config {

    @Bean
    public Engine engine() {
        return new Engine("v8", 5);
    }

    @Bean
    public Transmission transmission() {
        return new Transmission("sliding");
    }
}
```

And then inject these dependencies via constructor:

```java
@Component
public class Car {

    @Autowired
    public Car(Engine engine, Transmission transmission) {
        this.engine = engine;
        this.transmission = transmission;
    }
}
```

Constructor injection has a few advantages compared to field injection.

**The first benefit is testability.** 
Additionally, **with** **field injection, we can’t enforce class-level invariants,** so it’s possible to have a _UserService_ instance without a properly initialized _userRepository_. Therefore, we may experience random _NullPointerException_ here and there. Also, with constructor injection, it’s easier to build immutable components.

Moreover, **using constructors to create object instances is more natural from the OOP standpoint.**

On the other hand, the main disadvantage of constructor injection is its verbosity, especially when a bean has a handful of dependencies. Sometimes it can be a blessing in disguise, as we may try harder to keep the number of dependencies minimal.
### Setter-Based Dependency Injection
For setter-based DI, the container will call setter methods of our class after invoking a no-argument constructor or no-argument static factory method to instantiate the bean. Let’s create this configuration using annotations:

```java
@Bean
public Store store() {
    Store store = new Store();
    store.setItem(item1());
    return store;
}
```

### Field-Based Dependency Injection

In case of Field-Based DI, we can inject the dependencies by marking them with an _@Autowired_ annotation:

```java
public class Store {
    @Autowired
    private Item item; 
}
```

This approach might look simpler and cleaner, but we don’t recommend using it because it has a few drawbacks such as:

- This method uses reflection to inject the dependencies, which is costlier than constructor-based or setter-based injection.
- It’s really easy to keep adding multiple dependencies using this approach. If we were using constructor injection, having multiple arguments would make us think that the class does more than one thing, which can violate the Single Responsibility Principle.
