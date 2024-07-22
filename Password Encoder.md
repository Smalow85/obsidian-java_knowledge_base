Spring Security offers a range of password encoders to secure user passwords effectively. Let’s explore these encoders to understand their strengths and limitations .

```java
@Bean
public PasswordEncoder encoder() {
    return new BCryptPasswordEncoder();
}
```

BCrypt, however, **will internally generate a random salt** instead. This is important to understand because it means that each call will have a different result, so we only need to encode the password once.

## Encode the Password on Registration

```java
@Autowired
private PasswordEncoder passwordEncoder;

@Override
public User registerNewUserAccount(UserDto accountDto) throws EmailExistsException {
    if (emailExist(accountDto.getEmail())) {
        throw new EmailExistsException(
          "There is an account with that email adress:" + accountDto.getEmail());
    }
    User user = new User();
    user.setFirstName(accountDto.getFirstName());
    user.setLastName(accountDto.getLastName());
    
    user.setPassword(passwordEncoder.encode(accountDto.getPassword()));
    
    user.setEmail(accountDto.getEmail());
    user.setRole(new Role(Integer.valueOf(1), user));
    return repository.save(user);
}
```

## Encode the Password on Authentication

Now we’ll handle the other half of this process and encode the password when the user authenticates. First, we need to inject the password encoder bean we defined earlier into our authentication provider:

```java
@Autowired
private UserDetailsService userDetailsService;

@Bean
public DaoAuthenticationProvider authProvider() {
    DaoAuthenticationProvider authProvider = new DaoAuthenticationProvider();
    authProvider.setUserDetailsService(userDetailsService);
    authProvider.setPasswordEncoder(encoder());
    return authProvider;
}
```

```java
@Configuration
@ComponentScan(basePackages = { "com.baeldung.security" })
@EnableWebSecurity
public class SecSecurityConfig {

    @Bean
    public AuthenticationManager authManager(HttpSecurity http) throws Exception {
        return http.getSharedObject(AuthenticationManagerBuilder.class)
            .authenticationProvider(authProvider())
            .build();
    }
    
    ...
}
```

