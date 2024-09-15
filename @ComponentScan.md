When working with Spring, we can annotate our classes in order to make them into Spring beans. Furthermore, **we can tell Spring where to search for these annotated classes,** as not all of them must become beans in this particular run.

With Spring, we use the `@ComponentScan` annotation along with the [[@Configuration]] annotation to specify the packages that we want to be scanned. `@ComponentScan` without arguments tells Spring to scan the current package and all of its sub-packages.

```java
@Configuration
@ComponentScan
public class SpringComponentScanApp {
    private static ApplicationContext applicationContext;

    @Bean
    public ExampleBean exampleBean() {
        return new ExampleBean();
    }

    public static void main(String[] args) {
        applicationContext = 
          new AnnotationConfigApplicationContext(SpringComponentScanApp.class);

        for (String beanName : applicationContext.getBeanDefinitionNames()) {
            System.out.println(beanName);
        }
    }
}
```

### Using `@ComponentScan` in a Spring Boot Application

In Spring Boot is that many things happen implicitly. We use [[@SpringBootApplication]] annotation, but it’s a combination of three annotations:

```java
@Configuration
@EnableAutoConfiguration
@ComponentScan
```

### `@ComponentScan` for Specific Packages

```java
@ComponentScan(basePackages = "com.baeldung.componentscan.springapp.animals")
@Configuration
public class SpringComponentScanApp {
   // ...
}
```

Spring provides a convenient way to specify multiple package names. To do so, we need to use a string array. Each string of the array denotes a package name:

```java
@ComponentScan(basePackages = {"com.baeldung.componentscan.springapp.animals", "com.baeldung.componentscan.springapp.flowers"})
```

Another way is to use a filter, specifying the pattern for the classes to exclude:

```java
@ComponentScan(excludeFilters = 
  @ComponentScan.Filter(type=FilterType.REGEX,
    pattern="com\\.baeldung\\.componentscan\\.springapp\\.flowers\\..*"))
```

