When it comes to authentication and Spring Security you have roughly three scenarios:

1. The **default**: You _can_ access the (hashed) password of the user, because you have his details (username, password) saved in e.g. a database table.
2. **Less common**: You _cannot_ access the (hashed) password of the user. This is the case if your users and passwords are stored _somewhere_ else, like in 3rd party identity management product offering REST services for authentication. Example: Atlassian Crowd
3. **Also popular**: You want to use OAuth2 or "Login with Google/Twitter/etc." (OpenID), likely in combination with JWT. Then none of the following applies and you should go straight to the OAuth2.
### Basic Authentication with Spring

We can configure Spring Security using Java config:

```java
@Configuration
@EnableWebSecurity
public class CustomWebSecurityConfigurerAdapter {

    @Autowired 
    private RestAuthenticationEntryPoint authenticationEntryPoint;

    @Autowired
    public void configureGlobal(AuthenticationManagerBuilder auth) throws Exception {
        auth
          .inMemoryAuthentication()
          .withUser("user1")
          .password(passwordEncoder().encode("user1Pass"))
          .authorities("ROLE_USER");
    }

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http.authorizeHttpRequests(expressionInterceptUrlRegistry ->
                        expressionInterceptUrlRegistry.requestMatchers("/securityNone").permitAll()
                                .anyRequest().authenticated())
            .httpBasic(httpSecurityHttpBasicConfigurer -> httpSecurityHttpBasicConfigurer.authenticationEntryPoint(authenticationEntryPoint));
        http.addFilterAfter(new CustomFilter(), BasicAuthenticationFilter.class);
        return http.build();
    }
    
    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
}
```

Here we’re using the _httpBasic()_ element to define Basic Authentication inside the _SecurityFilterChain_ bean.

In this case Spring Security needs you to define two beans to get authentication up and running.
1. `UserDetailsService`
2. `PasswordEncoder`

Specifying a `UserDetailsService` is as simple as this:

```java
@Bean
public UserDetailsService userDetailsService() {
    return new MyDatabaseUserDetailsService(); // (1)
}
```

1. `MyDatabaseUserDetailsService` implements `UserDetailsService`, a very simple interface, which consists of one method returning a `UserDetails` object:

```java
public class MyDatabaseUserDetailsService implements UserDetailsService {

	UserDetails loadUserByUsername(String username) throws UsernameNotFoundException { // (1)
         // 1. Load the user from the users table by username. If not found, throw UsernameNotFoundException.
         // 2. Convert/wrap the user to a UserDetails object and return it.
        return someUserDetails;
    }
}

public interface UserDetails extends Serializable { // (2)

    String getUsername();

    String getPassword();

    // <3> more methods:
    // isAccountNonExpired,isAccountNonLocked,
    // isCredentialsNonExpired,isEnabled
}
```

1. `UserDetailsService` loads `UserDetails` via the user’s username. Note that the method takes **only** one parameter: username (not the password).
2. The `UserDetails` interface has methods to get the (hashed!) password and one to get the username.
3. `UserDetails` has even more methods, like is the account active or blocked, have the credentials expired or what permissions the user has - but we won’t cover them here.

You can always implement the `UserDetailsService` and `UserDetails` interfaces yourself.

But, you’ll also find off-the-shelf implementations by Spring Security that you can use/configure/extend/override instead.

1. **JdbcUserDetailsManager**, which is a JDBC(database)-based UserDetailsService. You can configure it to match your _user_ table/column structure.
2. **InMemoryUserDetailsManager**, which keeps all userdetails in-memory and is great for testing.
3. **org.springframework.security.core.userdetail.User**, which is a sensible, default UserDetails implementation that you could use. That would mean potentially mapping/copying between your entities/database tables and this user class. Alternatively, you could simply make your entities implement the UserDetails interface.

#### Full UserDetails Workflow: HTTP Basic Authentication

Now think back to your **HTTP Basic Authentication**, that means you are securing your application with Spring Security and Basic Auth. This is what happens when you specify a `UserDetailsService` and try to login:

1. Extract the username/password combination from the HTTP Basic Auth header in a filter. You don’t have to do anything for that, it will happen under the hood.
2. Call _your_ MyDatabaseUserDetailsService to load the corresponding user from the database, wrapped as a UserDetails object, which exposes the user’s hashed password.
3. Take the extracted password from the HTTP Basic Auth header, hash it _automatically_ and compare it with the hashed password from your UserDetails object. If both match, the user is successfully authenticated.

#### PasswordEncoders

Spring Security cannot magically guess your preferred password hashing algorithm. That’s why you need to specify another @Bean, a _PasswordEncoder_. If you want to, say, use the BCrypt password hashing function (Spring Security’s default) for _all your passwords_, you would specify this @Bean in your SecurityConfig.

```java
@Bean
public BCryptPasswordEncoder bCryptPasswordEncoder() {
    return new BCryptPasswordEncoder();
}
```

