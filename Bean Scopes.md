The latest version of the [[Spring Framework]] defines 6 types of scopes:

- singleton
- prototype
- request
- session
- application
- websocket

## Singleton

When we define a bean with the _singleton_ scope, the container creates a single instance of that bean; all requests for that bean name will return the same object, which is cached. Any modifications to the object will be reflected in all references to the bean. This scope is the default value if no other scope is specified.

```java
@Bean
@Scope("singleton")
public Person personSingleton() {
    return new Person();
}
```

## Prototype Scope

A bean with the _prototype_ scope will return a different instance every time it is requested from the container. It is defined by setting the value _prototype_ to the _@Scope_ annotation in the bean definition:

```java
@Bean
@Scope("prototype")
public Person personPrototype() {
    return new Person();
}
```

## Web Aware Scopes

### 1. Request Scope

We can define the bean with the _request_ scope using the _@Scope_ annotation:

```java
@Bean
@Scope(value = WebApplicationContext.SCOPE_REQUEST, proxyMode = ScopedProxyMode.TARGET_CLASS)
public HelloMessageGenerator requestScopedBean() {
    return new HelloMessageGenerator();
}
```

The _proxyMode_ attribute is necessary because at the moment of the instantiation of the web application context, there is no active request. Spring creates a proxy to be injected as a dependency, and instantiates the target bean when it is needed in a request.

We can also use a _@RequestScope_ composed annotation that acts as a shortcut for the above definition:

```java
@Bean
@RequestScope
public HelloMessageGenerator requestScopedBean() {
    return new HelloMessageGenerator();
}
```

If we display the _message_ each time the request is run, we can see that the value is reset to _null_, even though it is later changed in the method. This is because of a different bean instance being returned for each request.
### 2. Session Scope

We can define the bean with the _session_ scope in a similar manner:

```java
@Bean
@Scope(value = WebApplicationContext.SCOPE_SESSION, proxyMode = ScopedProxyMode.TARGET_CLASS)
public HelloMessageGenerator sessionScopedBean() {
    return new HelloMessageGenerator();
}
```

There’s also a dedicated composed annotation we can use to simplify the bean definition:

```java
@Bean
@SessionScope
public HelloMessageGenerator sessionScopedBean() {
    return new HelloMessageGenerator();
}
```

In this case, when the request is made for the first time, the value _message_ is _null._ However, once it is changed, that value is retained for subsequent requests as the same instance of the bean is returned for the entire session.

### 3. Application Scope

The _application_ scope creates the bean instance for the lifecycle of a _ServletContext._

This is similar to the _singleton_ scope, but there is a very important difference with regards to the scope of the bean.

When beans are _application_ scoped, the same instance of the bean is shared across multiple servlet-based applications running in the same _ServletContext_, while _singleton_ scoped beans are scoped to a single application context only.

```java
@Bean
@Scope(
  value = WebApplicationContext.SCOPE_APPLICATION, proxyMode = ScopedProxyMode.TARGET_CLASS)
public HelloMessageGenerator applicationScopedBean() {
    return new HelloMessageGenerator();
}
```

Analogous to the _request_ and _session_ scopes, we can use a shorter version:

```java
@Bean
@ApplicationScope
public HelloMessageGenerator applicationScopedBean() {
    return new HelloMessageGenerator();
}
```

### 4. WebSocket Scope

Finally, let’s create the bean with the _websocket_ scope:

```java
@Bean
@Scope(scopeName = "websocket", proxyMode = ScopedProxyMode.TARGET_CLASS)
public HelloMessageGenerator websocketScopedBean() {
    return new HelloMessageGenerator();
}
```

When first accessed, _WebSocket_ scoped beans are stored in the _WebSocket_ session attributes. The same instance of the bean is then returned whenever that bean is accessed during the entire _WebSocket_ session.

