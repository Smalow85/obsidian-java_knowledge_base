Cookies are pieces of data used to identify the user and their preferences. The browser returns the cookie to the server every time the page is requested. Specific cookies like HTTP cookies are used to perform cookie-based authentication to maintain the session for each user.

Cookie [[Authentication (API)]] uses [[HTTP cookies]] to authenticate client requests and maintain session information. It works as follows:

1. The client sends a login request to the server.
2. On the successful login, the server response includes the `Set-Cookie` header that contains the cookie name, value, expiry time and some other info. Here is an example that sets the cookie named `JSESSIONID`:

```
Set-Cookie: JSESSIONID=abcde12345; Path=/; HttpOnly`
```
    
3. The client needs to send this cookie in the `Cookie` header in all subsequent requests to the server.
    
```
Cookie: JSESSIONID=abcde12345`
```
    
4. On the logout operation, the server sends back the `Set-Cookie` header that causes the cookie to expire.