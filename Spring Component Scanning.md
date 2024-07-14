When working with [[Spring Framework]], we can annotate our classes in order to make them into Spring beans. Furthermore, **we can tell Spring where to search for these annotated classes,** as not all of them must become beans in this particular run.

## @ComponentScan Without Arguments

_@ComponentScan_ without arguments tells Spring to scan the current package and all of its sub-packages.

We should also note that the main application class and the configuration class are not necessarily the same. If they are different, it doesn’t matter where we put the main application class. **Only the location of the configuration class matters, as component scanning starts from its package by default**.

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

## @ComponentScan With Arguments

First, we can change the base package:

```java
@ComponentScan(basePackages = "com.baeldung.componentscan.springapp.animals")
@Configuration
public class SpringComponentScanApp {
   // ...
}
```

Spring provides a convenient way to specify multiple package names. To do so, we need to use a string array.

Each string of the array denotes a package name:

```java
@ComponentScan(basePackages = {"com.baeldung.componentscan.springapp.animals", "com.baeldung.componentscan.springapp.flowers"})
```

Another way is to use a filter, specifying the pattern for the classes to exclude:

```java
@ComponentScan(excludeFilters = 
  @ComponentScan.Filter(type=FilterType.REGEX,
    pattern="com\\.baeldung\\.componentscan\\.springapp\\.flowers\\..*"))
```




