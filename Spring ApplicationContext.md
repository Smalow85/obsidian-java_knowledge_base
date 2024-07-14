One of the main features of the [[Spring Framework]] is the **IoC** (Inversion of Control) container. The Spring IoC container is responsible for managing the objects of an application. It uses dependency injection to achieve inversion of control.

The interfaces _[BeanFactory](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/BeanFactory.html)_ and _[ApplicationContext](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/ApplicationContext.html)_ **represent the Spring IoC container**. Here, _BeanFactory_ is the root interface for accessing the Spring container. It provides basic functionalities for managing beans.

On the other hand, the _ApplicationContext_ is a sub-interface of the _BeanFactory_. Therefore, it offers all the functionalities of _BeanFactory._

Furthermore, it **provides** **more enterprise-specific functionalities**. The important features of _ApplicationContext_ are **resolving messages, supporting internationalization, publishing events, and application-layer specific contexts**. This is why we use it as the default Spring container.
## What Is a Spring Bean?

Before we dive deeper into the _ApplicationContext_ container, it’s important to know about Spring beans. [[Spring Bean]] is **an object that the Spring container instantiates, assembles, and manages**.

## Configuring Beans in the Container

### 1. Java-Based Configuration

Java configuration typically uses _@Bean_-annotated methods within a _@Configuration_ class. The _@Bean_ annotation on a method indicates that the method creates a Spring bean. Moreover, a class annotated with _@Configuration_ indicates that it contains Spring bean configurations.

```java
@Configuration
public class AccountConfig {

  @Bean
  public AccountService accountService() {
    return new AccountService(accountRepository());
  }

  @Bean
  public AccountRepository accountRepository() {
    return new AccountRepository();
  }
}
```

### 2. Annotation-Based Configuration

In this approach, we first enable annotation-based configuration via XML configuration. Then we use a set of annotations on our Java classes, methods, constructors, or fields to configure beans. Some examples of these annotations are _@Component_, _@Controller_, _@Service_, _@Repository_, _@Autowired_, and _@Qualifier_.

Notably, we use these annotations with Java-based configuration as well. Also worth mentioning, Spring keeps on adding more capabilities to these annotations with each release.

First, we’ll create the XML configuration, _user-bean-config.xml_, to enable annotations:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:context="http://www.springframework.org/schema/context"
  xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context.xsd">
  
  <context:annotation-config/>
  <context:component-scan base-package="com.baeldung.applicationcontext"/>

</beans>
```

Here, the _annotation-config_ tag enables annotation-based mappings. The _component-scan_ tag also tells Spring where to look for annotated classes.

Second, we’ll create the _UserService_ class and define it as a Spring bean using the _@Component_ annotation:

```java
@Component
public class UserService {
  // user service code
}
```

### 3. XML-Based Configuration

This is the traditional way of configuring beans in Spring. Obviously, in this approach, we do all **bean mappings in an XML configuration file**.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="
    http://www.springframework.org/schema/beans 
    http://www.springframework.org/schema/beans/spring-beans.xsd">
	  
  <bean id="accountService" class="com.baeldung.applicationcontext.AccountService">
    <constructor-arg name="accountRepository" ref="accountRepository" />
  </bean>
	
  <bean id="accountRepository" class="com.baeldung.applicationcontext.AccountRepository" />
</beans>
```

## Types of ApplicationContext

Spring provides different types of _ApplicationContext_ containers suitable for different requirements. These are implementations of the _ApplicationContext_ interface. So let’s take a look at some of the common types of _ApplicationContext_.

### 1. AnnotationConfigApplicationContext
 It can take classes annotated with _@Configuration_, _@Component,_ and JSR-330 metadata as input.
### 2. AnnotationConfigWebApplicationContext
It is a web-based variant of _AnnotationConfigApplicationContext_. We may use this class when we configure Spring’s _ContextLoaderListener_ servlet listener or a Spring MVC _DispatcherServlet_ in a _web.xml_ file. 

Moreover, from Spring 3.0 onward, we can also configure this application context container programmatically. All we need to do is implement the [_WebApplicationInitializer_](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/web/WebApplicationInitializer.html) interface:

```java
public class MyWebApplicationInitializer implements WebApplicationInitializer {

  public void onStartup(ServletContext container) throws ServletException {
    AnnotationConfigWebApplicationContext context = new AnnotationConfigWebApplicationContext();
    context.register(AccountConfig.class);
    context.setServletContext(container);

    // servlet configuration
  }
}
```

### 3. XmlWebApplicationContext
If we use the **XML based configuration in a web application**, we can use the [_XmlWebApplicationContext_](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/web/context/support/XmlWebApplicationContext.html) class.

As a matter of fact, configuring this container is like the _AnnotationConfigWebApplicationContext_ class only, which means we can configure it in _web.xml,_ or implement the _WebApplicationInitializer_ interface:

```java
public class MyXmlWebApplicationInitializer implements WebApplicationInitializer {

  public void onStartup(ServletContext container) throws ServletException {
    XmlWebApplicationContext context = new XmlWebApplicationContext();
    context.setConfigLocation("/WEB-INF/spring/applicationContext.xml");
    context.setServletContext(container);

    // Servlet configuration
  }
}
```

### 4. FileSystemXMLApplicationContext

We use the [_FileSystemXMLApplicationContext_](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/support/FileSystemXmlApplicationContext.html) class to **load an XML-based Spring configuration file from the file system** or from URLs. This class is useful when we need to load the _ApplicationContext_ programmatically.

### 5. ClassPathXmlApplicationContext
In case we want to **load an XML configuration file from the classpath**, we can use the [_ClassPathXmlApplicationContext_](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/support/ClassPathXmlApplicationContext.html) class. Similar to _FileSystemXMLApplicationContext,_ it’s useful for test harnesses, as well as application contexts embedded within JARs.
```java
ApplicationContext context = new ClassPathXmlApplicationContext("applicationcontext/account-bean-config.xml");
AccountService accountService = context.getBean("accountService", AccountService.class);
```


