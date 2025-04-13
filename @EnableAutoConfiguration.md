This annotation signals to Spring Boot to start adding beans based on classpath settings, other beans, and various property settings. When you use this annotation, Spring Boot attempts to auto-configure beans that you are likely to need.

Behind the scenes, `@EnableAutoConfiguration` leverages the Spring Factories Loader mechanism to locate and load configurations.

Spring Boot checks the classpath for `META-INF/spring.factories` files. Inside this file, it looks for the key `org.springframework.boot.autoconfigure.EnableAutoConfiguration` and gets a list of all classes under this key.

## Exclude Auto-configuration Classes

One of the ways to customize the auto-configuration process is by excluding specific auto-configuration classes that you don’t want to be applied. You can do this using the `exclude` attribute of `@EnableAutoConfiguration`:

```java
@EnableAutoConfiguration(exclude = DataSourceAutoConfiguration.class)  
@ComponentScan(basePackages = "com.example.myapp")  
public class MyApp {  
    public static void main(String[] args) {  
        SpringApplication.run(MyApp.class, args);  
    }  
}
```

## Overriding Auto-configuration

Another way to customize is by declaring your beans, which overrides the beans defined in auto-configuration.

For instance, if you define your `DataSource` bean, it will override the auto-configured one.

```java
@Configuration  
public class MyConfiguration {  
    @Bean  
    public DataSource dataSource() {  
        // return your custom datasource  
    }  
}
```