## 1. What is Spring AOP?

Aspect-Oriented Programming is a paradigm that complements Object-Oriented Programming (OOP). While OOP is concerned with organizing code into classes and objects, AOP focuses on cross-cutting concerns – functionalities that affect multiple parts of an application. Cross-cutting concerns include logging, security, transactions, and more.

In the [[Spring Framework]], Spring AOP is a core module that provides a robust and elegant solution to address the challenges posed by cross-cutting concerns. It allows us to modularize these concerns thus enhancing code maintainability and reducing redundancy. Spring AOP seamlessly integrates with the Spring IoC ([Inversion of Control](https://howtodoinjava.com/spring-core/spring-ioc-vs-di/)) container, making it an integral part of the Spring ecosystem.

## 2. Spring AOP Architecture

The core architecture of Spring AOP is based on _proxies_. When the application is initialized, an advised instance of a class is created as the result of a _ProxyFactory_ creating a proxy instance of that class with all the aspects woven into the proxy.

In runtime, Spring analyzes the crosscutting concerns defined for the beans in _ApplicationContext_ and generates proxy beans (which wrap the underlying target bean) dynamically. Instead of accessing the target bean directly, callers are injected with the proxied bean.

![[Pasted image 20240714223150.png]]

Internally, Spring has two proxy implementations:

- **JDK dynamic proxies**: when the target object to be advised implements an interface.
- **CGLIB proxies**: when the advised target object doesn’t implement an interface. For example, it’s a concrete class.

Remember that **Spring AOP has some limitations**. Such as:

- Final classes or methods cannot be proxied since they cannot be extended.
- Also, due to the proxy implementation, Spring AOP only applies to public, nonstatic methods on Spring Beans.
- If there is an internal method call from one method to another within the same class, the advice will never be executed for the internal method call.

## 3. What is Advice, Joinpoint and Pointcut?

- **Joinpoint**: is a point of execution of the program, such as executing a method or handling an exception. **In Spring AOP, a joinpoint always represents a method execution.** Joinpoints define the points in your application at which you can insert additional logic using AOP.
- **Advice**: is the code that is executed at a particular joinpoint. There are many types of advice, such as _before_, which executes before the joinpoint, and _after_, which executes after it.
- **Aspect**: is the combination of advices and pointcuts encapsulated in a class.
- **Pointcut**: is a predicate or expression that matches joinpoints.
- **Weaving**: is the process of inserting aspects into the application code at the appropriate point. AspectJ supports a weaving mechanism called _loadtime weaving (LTW)_, in which it intercepts the underlying JVM class loader and provides weaving to the bytecode when it is being loaded by the class loader.
- **Target**: is the object whose execution flow is modified by an AOP process. Often you see the target object referred to as the _advised object_.
- Spring uses the AspectJ pointcut expression language by default.

![Spring AOP](https://howtodoinjava.com/wp-content/uploads/2015/01/spring-aop-diagram.jpg)

