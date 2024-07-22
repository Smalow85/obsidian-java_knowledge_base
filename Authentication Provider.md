Spring Security provides a variety of options for performing authentication. These options follow a simple contract: an _AuthenticationProvider_ processes an _Authentication_ request, and a fully authenticated object with full credentials is returned.

The standard and most common implementation is the _DaoAuthenticationProvider,_ which retrieves the user details from a simple, read-only user DAO, the _UserDetailsService_. This User Details Service **only has access to the username** in order to retrieve the full user entity, which is enough for most scenarios.

More custom scenarios will still need to access the full _Authentication_ request to be able to perform the authentication process. For example, when authenticating against some external, third-party service (such as [Crowd](https://www.atlassian.com/software/crowd "Crowd - Identity Management")), both the _username_ and _password_ from the authentication request will be necessary.

For these more advanced scenarios, we’ll need to **define a custom Authentication Provider**:

```java
@Component
public class CustomAuthenticationProvider implements AuthenticationProvider {

    @Override
    public Authentication authenticate(final Authentication authentication) throws AuthenticationException {
        final String name = authentication.getName();
        final String password = authentication.getCredentials().toString();
        if (!"admin".equals(name) || !"system".equals(password)) {
            return null;
        }
        return authenticateAgainstThirdPartyAndGetAuthentication(name, password);
    }

    @Override
    public boolean supports(Class<?> authentication) {
        return authentication.equals(UsernamePasswordAuthenticationToken.class);
    }
}
```

1. Compared to the UserDetails load() method, where you only had access to the username, you now have access to the complete authentication attempt, _usually_ containing a username and password.
2. You can do whatever you want to authenticate the user, e.g. call a REST-service.
3. If authentication failed, you need to throw an exception.
4. If authentication succeeded, you need to return a fully initialized UsernamePasswordAuthenticationToken. It is an implementation of the Authentication interface and needs to have the field authenticated be set to true (which the constructor used above will automatically set). We’ll cover authorities in the next chapter.