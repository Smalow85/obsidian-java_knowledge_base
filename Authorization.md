`Authentication`discusses how all `Authentication` implementations store a list of `GrantedAuthority` objects. These represent the authorities that have been granted to the principal. The `GrantedAuthority` objects are inserted into the `Authentication` object by the `AuthenticationManager` and are later read by `AccessDecisionManager` instances when making authorization decisions.

The `GrantedAuthority` interface has only one method:

```java
String getAuthority();
```

This method is used by an `AuthorizationManager` instance to obtain a precise `String` representation of the `GrantedAuthority`. By returning a representation as a `String`, a `GrantedAuthority` can be easily "read" by most `AuthorizationManager` implementations. If a `GrantedAuthority` cannot be precisely represented as a `String`, the `GrantedAuthority` is considered "complex" and `getAuthority()` must return `null`. This indicates to any `AuthorizationManager` that it needs to support the specific `GrantedAuthority` implementation to understand its contents.
## Invocation Handling

Spring Security provides interceptors that control access to secure objects, such as method invocations or web requests. A pre-invocation decision on whether the invocation is allowed to proceed is made by `AuthorizationManager` instances. Also post-invocation decisions on whether a given value may be returned is made by `AuthorizationManager` instances.
### AuthorizationManager

`AuthorizationManager` supersedes both `AccessDecisionManager` and `AccessDecisionVoter`.

`AuthorizationManager` are called by Spring Security’s [request-based](https://docs.spring.io/spring-security/reference/servlet/authorization/authorize-http-requests.html), [method-based](https://docs.spring.io/spring-security/reference/servlet/authorization/method-security.html), and [message-based](https://docs.spring.io/spring-security/reference/servlet/integrations/websocket.html) authorization components and are responsible for making final access control decisions. The `AuthorizationManager` interface contains two methods:

```java
AuthorizationDecision check(Supplier<Authentication> authentication, Object secureObject);

default AuthorizationDecision verify(Supplier<Authentication> authentication, Object secureObject)
        throws AccessDeniedException {
    // ...
}
```

Implementations are expected to return a positive `AuthorizationDecision` if access is granted, negative `AuthorizationDecision` if access is denied, and a null `AuthorizationDecision` when abstaining from making a decision.

`verify` calls `check` and subsequently throws an `AccessDeniedException` in the case of a negative `AuthorizationDecision`.

Whilst users can implement their own `AuthorizationManager` to control all aspects of authorization, Spring Security ships with a delegating `AuthorizationManager` that can collaborate with individual `AuthorizationManager`s.

![[Pasted image 20240715203445.png]]

## Legacy Authorization Components

### AccessDecisionManager

The `AccessDecisionManager` is called by the `AbstractSecurityInterceptor` and is responsible for making final access control decisions. The `AccessDecisionManager` interface contains three methods:

```java
void decide(Authentication authentication, Object secureObject,
	Collection<ConfigAttribute> attrs) throws AccessDeniedException;

boolean supports(ConfigAttribute attribute);

boolean supports(Class clazz);
```

The `decide` method of the `AccessDecisionManager` is passed all the relevant information it needs to make an authorization decision. In particular, passing the secure `Object` lets those arguments contained in the actual secure object invocation be inspected. For example, assume the secure object is a `MethodInvocation`. You can query the `MethodInvocation` for any `Customer` argument and then implement some sort of security logic in the `AccessDecisionManager` to ensure the principal is permitted to operate on that customer. Implementations are expected to throw an `AccessDeniedException` if access is denied.
### Voting-Based AccessDecisionManager Implementations

While users can implement their own `AccessDecisionManager` to control all aspects of authorization, Spring Security includes several `AccessDecisionManager` implementations that are based on voting. [Voting Decision Manager](https://docs.spring.io/spring-security/reference/servlet/authorization/architecture.html#authz-access-voting) describes the relevant classes.

The following image shows the `AccessDecisionManager` interface:

![access decision voting](https://docs.spring.io/spring-security/reference/_images/servlet/authorization/access-decision-voting.png)

By using this approach, a series of `AccessDecisionVoter` implementations are polled on an authorization decision. The `AccessDecisionManager` then decides whether or not to throw an `AccessDeniedException` based on its assessment of the votes.

# Spring Security Expressions

Now let’s explore the security expressions:

- _hasRole_, _hasAnyRole_
- _hasAuthority_, _hasAnyAuthority_
- _permitAll_, _denyAll_
- _isAnonymous_, _isRememberMe_, _isAuthenticated_, _isFullyAuthenticated_
- _principal_, _authentication_
- _hasPermission_

### 1. hasRole, hasAnyRole

These expressions are responsible for defining the access control or authorization to specific URLs and methods in our application:

```java
@Bean
public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
    ...
    .antMatchers("/auth/admin/*").hasRole("ADMIN")
    .antMatchers("/auth/*").hasAnyRole("ADMIN","USER")
    ...
}
```

### 2. hasAuthority, hasAnyAuthority

Roles and authorities are similar in Spring. The main difference is that roles have special semantics. Starting with Spring Security 4, the ‘_ROLE_‘ prefix is automatically added (if it’s not already there) by any role-related method.

So _hasAuthority(‘ROLE_ADMIN’)_ is similar to _hasRole(‘ADMIN’)_ because the _ROLE_ prefix gets added automatically.

Here’s a quick example defining users with specific authorities:

```java
@Bean
public InMemoryUserDetailsManager userDetailsService() {
    UserDetails admin = User.withUsername("admin")
        .password(encoder().encode("adminPass"))
        .roles("ADMIN")
        .build();
    UserDetails user = User.withUsername("user")
        .password(encoder().encode("userPass"))
        .roles("USER")
        .build();
    return new InMemoryUserDetailsManager(admin, user);
}
```

### 3. permitAll, denyAll

These two annotations are also quite straightforward. We may either permit or deny access to some URL in our service.

```java
...
.antMatchers("/*").permitAll()
...
```

```java
...
.antMatchers("/*").denyAll()
...
```

### 4. isAnonymous, isRememberMe, isAuthenticated, isFullyAuthenticated

By specifying the following in the Java config, we’ll enable all unauthorized users to access our main page:

```java
...
.antMatchers("/*").anonymous()
...
```

If we want to secure the website so that everyone who uses it needs to log in, we’ll need to use the _isAuthenticated()_ method:

```java
...
.antMatchers("/*").authenticated()
...
```

In order to give access to users that were logged in by the remember me function, we can use:

```java
...
.antMatchers("/*").rememberMe()
...
```

We can specify _isFullyAuthenticated()_, which returns _true_ if the user isn’t an anonymous or remember-me user:

```java
...
.antMatchers("/*").fullyAuthenticated()
...
```

## Check If a User Has a Role in Java

### @PreAuthorize

```java
@PreAuthorize("hasRole('ROLE_ADMIN')")
@GetMapping("/user/{id}")
public String getUser(@PathVariable("id") String id) {
    ...
}
```

We can also check multiple roles in a single expression:

```java
@PreAuthorize("hasAnyRole('ROLE_ADMIN','ROLE_MANAGER')")
@GetMapping("/users")
public String getUsers() {
    ...
}
```

### 2. SecurityContext

```java
Authentication auth = SecurityContextHolder.getContext().getAuthentication();
if (auth != null && auth.getAuthorities().stream().anyMatch(a -> a.getAuthority().equals("ADMIN"))) {
    ...
}
```

### 3. UserDetailsService

```java
@GetMapping("/users")
public String getUsers() {
    UserDetails details = userDetailsService.loadUserByUsername("mike");
    if (details != null && details.getAuthorities().stream()
      .anyMatch(a -> a.getAuthority().equals("ADMIN"))) {
        // ...
    }
}
```

### 4. Servlet Request

```java
@GetMapping("/users")
public String getUsers(HttpServletRequest request) {
    if (request.isUserInRole("ROLE_ADMIN")) {
        ...
    }
}
```