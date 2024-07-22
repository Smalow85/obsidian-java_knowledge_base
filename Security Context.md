The **SecurityContext** and **SecurityContextHolder** are two fundamental classes of Spring Security. The `SecurityContext` is used to store the details of the currently authenticated user, also known as a principle.

The `SecurityContextHolder` is a helper class, which provide access to the security context. The key thing to learn is that _how do you get the SecurityContext from the SecurityContextHolder?_ and then retrieving current user details from that? For example, if you want to know the username of the current logged in user then how do you get that in Spring security?

This `SecurityContext` keep the user details in an Authentication object, which can be obtained by calling `getAuthentication()` method.

```java
Object principal = SecurityContextHolder.getContext().getAuthentication().getPrincipal();

if (principal instanceof UserDetails) {
	String username = ((UserDetails)principal).getUsername();
} else {
	String username = principal.toString();
}
```



