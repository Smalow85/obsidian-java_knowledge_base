- The annotation `@EnableAutoConfiguration` is used to enable the auto-configuration feature.
- The `@EnableAutoConfiguration` annotation enables the auto-configuration of Spring `ApplicationContext` by scanning the classpath components and registering the beans.
- This annotation is wrapped inside the `@SpringBootApplication` annotation along with `@ComponentScan` and `@SpringBootConfiguration` annotations.
- When running main() method, this annotation initiates auto-configuration.

```java
@SpringBootApplication
public class GfgApplication {
	public static void main(String[] args) {
		SpringApplication.run(GfgApplication.``class``, args);
	}
}
```

###  Working of Auto-Configuration in Spring Boot

##### 1. Dependencies

Auto-Configuration of dependencies is the main focus of the [[Spring Boot]] development.
- Our Spring application needs a respective set of dependencies to work.
- Spring Boot auto-configures a pre-set of the required dependencies without a need to configure them manually.
- This greatly helps and can be seen when we want to create a stand-alone application.
- When we build our application, Spring Boot looks after our dependencies and configures both the underlying Spring Framework and required jar dependencies (third-party libraries) on the classpath according to our project built.
- It helps us to avoid errors like mismatches or incompatible versions of different libraries.
- If you want to override these defaults, you can override them after initialization.

- When you build a Spring Boot project, the ‘Starter Parent’ dependency gets automatically added in the ‘pom.xml’ file.
- It notifies that the essential ‘sensible’ defaults for the application have been auto-configured and you therefore can take advantage of it.

```xml
<parent>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-parent</artifactId>
	<version>...</version>
</parent>
```

- To add the dependency (library of tech stacks), you don’t need to mention the version of it because the Spring Boot automatically configures it for you.
- Also, when you update/change the Spring Boot version, all the versions of added dependencies will also get updated/changed.

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

- It is Spring Boot’s auto-configuration that makes managing dependencies supremely easy for us.

##### 2. Application

1. `@Bean` is a method-level annotation.
2. `@Bean` annotation specifies that a method produces a return value registered as a bean  with BeanFactory – managed by Spring Container.
3. This particular java program uses `@Configuration` annotation specifying that the class contains one or more `@Bean` annotations which help to automatically register (initialize) in the Spring Container (Spring Application Context).
4. `@Configuration` is a class-level annotation.

```java
@Configuration`
public class ConfigDataSource {
	@Bean 
	public static DataSource source() {
		DataSourceBuilder<?> dSB = DataSourceBuilder.create();
		dSB.driverClassName("com.mysql.jdbc.Driver");
		dSB.url("jdbc:mysql://localhost:3306/userdetails");
		dSB.username("user");
		dSB.password("password");
		return dSB.build();
	}
}
```



