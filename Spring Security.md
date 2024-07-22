Spring Security is a sub-project of [[Spring Framework]], which provides various security features like: authentication, authorization to create secure Java Enterprise Applications.

Framework targets two areas - **authentication** and **authorization**. [[Authentication]] is the process of knowing and identifying the user that wants to access. [[Authorization]] is the process to allow authority to perform actions in the application.
## Advantages of Spring security

- Comprehensive support for authentication and authorization.
- Protection against common tasks
- Servlet API integration
- Integration with Spring MVC
- Portability
- CSRF protection
- Java Configuration support

## How to configure Spring Security: WebSecurityConfigurerAdapter

With the latest Spring Security and/or Spring Boot versions, the way to configure Spring Security is by having a class that:

1. Is annotated with @EnableWebSecurity
2. Extends WebSecurityConfigurer, which basically offers you a configuration DSL/methods. With those methods, you can specify what URIs in your application to protect or what exploit protections to enable/disable.

Here’s what a typical WebSecurityConfigurerAdapter looks like:

```java
@Configuration
@EnableWebSecurity // (1)
public class WebSecurityConfig extends WebSecurityConfigurerAdapter { // (1)

  @Override
  protected void configure(HttpSecurity http) throws Exception {  // (2)
      http
        .authorizeRequests()
          .antMatchers("/", "/home").permitAll() // (3)
          .anyRequest().authenticated() // (4)
          .and()
       .formLogin() // (5)
         .loginPage("/login") // (5)
         .permitAll()
         .and()
      .logout() // (6)
        .permitAll()
        .and()
      .httpBasic(); // (7)
  }
}
```

1. A normal Spring `@Configuration` with the `@EnableWebSecurity` annotation, extending from `WebSecurityConfigurerAdapter`.
2. By overriding the adapter’s configure(HttpSecurity) method, you get a nice little **DSL** with which you can configure your FilterChain.
3. All requests going to `_/_` and `_/home_` are allowed (permitted) - the user does _not_ have to authenticate. You are using an [antMatcher](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/util/AntPathMatcher.html), which means you could have also used wildcards (*, \*\*, ?) in the string.
4. Any other request needs the user to be authenticated _first_, i.e. the user needs to login.
5. You are allowing form login (username/password in a form), with a custom loginPage (`_/login_`, i.e. not Spring Security’s auto-generated one). Anyone should be able to access the login page, without having to log in first (permitAll; otherwise we would have a Catch-22!).
6. The same goes for the logout page
7. On top of that, you are also allowing Basic Auth, i.e. sending in an HTTP Basic Auth Header to authenticate.
## Spring Security Authentication Architecture

![[Pasted image 20240715114144.png]]

1. [[Authentication]]
2. [[Spring Security Filters]]
3. [[Security Context]]
4. [[Authentication Manager]]
5. [[Authentication Provider]]
6. UserDetailsService
7. [[Password Encoder]]

