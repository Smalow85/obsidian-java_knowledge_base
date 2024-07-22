
![[Pasted image 20240715113958.png]]

A series of Spring Security filters intercept each request & work together to identify if Authentication is required or not. If authentication is required, accordingly navigate the user to login page or use the existing details stored during initial authentication.

[[Spring Security]] maintains a filter chain internally where each of the filters has a particular responsibility and filters are added or removed from the configuration depending on which services are required. The ordering of the filters is important as there are dependencies between them.

![[Pasted image 20240715114727.png]]

Spring Security does not just install _one_ filter, instead it installs a whole filter chain consisting of 15 (!) different filters.

So, when an HTTPRequest comes in, it will go through _all_ these 15 filters, before your request finally hits your @RestControllers. The order is important, too, starting at the top of that list and going down to the bottom.

[![filterchain 1a](https://www.marcobehler.com/images/filterchain-1a.png)](https://www.marcobehler.com/images/filterchain-1a.png)
You can add any security chain expressions here like.

1. [**org.springframework.security.web.csrf.CsrfFilter**](https://docs.spring.io/spring-security/site/docs/4.0.x/apidocs/org/springframework/security/web/csrf/CsrfFilter.html) : This filter applies for CSRF protection by default to all REST endpoints.
2. [**org.springframework.security.web.authentication.logout.LogoutFilter**](https://docs.spring.io/spring-security/site/docs/4.0.x/apidocs/org/springframework/security/web/authentication/logout/LogoutFilter.html) : This filter gets called when the user logs out of the application. The default registered instances `LogoutHandler` are called that are responsible for invalidating the session and clearing the `SecurityContext`. Next, the default implementation of `LogoutSuccessHandler` redirects the user to a new page (`/login?logout`).
3. [**org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter**](https://docs.spring.io/spring-security/site/docs/current/api/org/springframework/security/web/authentication/UsernamePasswordAuthenticationFilter.html) : Validates the username and password for the URL (`/login`) with the default credentials provided at startup.
4. [**org.springframework.security.web.authentication.ui.DefaultLoginPageGeneratingFilter**](https://docs.spring.io/spring-security/site/docs/current/api/org/springframework/security/web/authentication/ui/DefaultLoginPageGeneratingFilter.html) : Generates the default login page html at `/login`
5. [**org.springframework.security.web.authentication.ui.DefaultLogoutPageGeneratingFilter**](https://docs.spring.io/spring-security/site/docs/current/api/org/springframework/security/web/authentication/ui/DefaultLogoutPageGeneratingFilter.html) : Generates the default logout page html at `/login?logout`
6. [**org.springframework.security.web.authentication.www.BasicAuthenticationFilter**](https://docs.spring.io/spring-security/site/docs/current/api/org/springframework/security/web/authentication/www/BasicAuthenticationFilter.html) : This filter is responsible for processing any request that has an HTTP request header of **Authorization**, **Basic Authentication scheme**, **Base64 encoded username-password**. On successful authentication, the `Authentication` object will be placed in the `SecurityContextHolder`.
7. [**org.springframework.security.web.authentication.AnonymousAuthenticationFilter**](https://docs.spring.io/spring-security/site/docs/4.0.x/apidocs/org/springframework/security/web/authentication/AnonymousAuthenticationFilter.html) : If no `Authentication` the object is found in the `SecurityContext`, it creates one with the principal `anonymousUser` and role `ROLE_ANONYMOUS`.
8. [**org.springframework.security.web.access.ExceptionTranslationFilter**](https://docs.spring.io/spring-security/site/docs/current/api/org/springframework/security/web/access/ExceptionTranslationFilter.html) : Handles `AccessDeniedException` and `AuthenticationException` thrown within the filter chain. For `AuthenticationException` instance of `AuthenticationEntryPoint` are required to handle responses. For, this filter will delegate to `AccessDeniedHandler` whose default implementation is `AccessDeniedHandlerImpl`.
9. [**org.springframework.security.web.access.intercept.FilterSecurityInterceptor**](https://docs.spring.io/spring-security/site/docs/6.0.0/api/org/springframework/security/web/access/intercept/FilterSecurityInterceptor.html) : This filter is responsible for authorizing every request that passes through the filter chain before the request hits the controller.

Example of security filter chain configuration:

![[Pasted image 20240715120432.png]]

## Creating custom filter

Spring Security provides a number of filters by default, and these are enough most of the time.

**But of course it’s sometimes necessary to implement new functionality by creating a new filter to use in the chain.**

Each position of the filter is an index (a number) and  one can add a new filter to the spring security chain either before, after, or at the position of a known one.

Below are the methods available to configure a custom filter in the spring security flow:

- **addFilterBefore(filter, class) —** adds a filter before the position of the specified filter class
- **addFilterAfter(filter, class) —** adds a filter after the position of the specified filter class.
- **addFilterAt(filter, class) —** adds a filter at the location of the specified filter class.

Now lets create a custom filter using addFilterAfter method which will simply log about the successful authentication and the authority details of the logged in user.

![](https://miro.medium.com/v2/resize:fit:1050/1*tCRXxOerZs3I9t0vAC1pdQ.png)

After creating the custom authentication filter we need to inject this filter in spring security filter chain. Please refer the below snapshot of code:

![](https://miro.medium.com/v2/resize:fit:1050/1*I9aIt1GNgBUrVQT116wMwA.png)

Here the addFilterAfter method takes two arguments in which the first one is the custom filter we need to invoke and the second argument we need to pass what is a Spring Security internal filter that we need to consider. So the internal filter that we are considering here is BasicAuthenticationFilter.class.

