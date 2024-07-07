Token-based authentication is a protocol which allows users to verify their identity, and in return receive a unique [[access token]]. During the life of the token, users then access the website or app that the token has been issued for, rather than having to re-enter credentials each time they go back to the same webpage, app, or any resource protected with that same token.

Token-based authentication is different from traditional password-based or server-based authentication techniques. Tokens offer a second layer of security, and administrators have detailed control over each action and transaction.
## Token Authentication in 4 Easy Steps

Use a token-based authentication system, and visitors will verify credentials just once. In return, they'll get a token that allows access for a time period you define.

The process works like this:

- **Request:** The person asks for access to a server or protected resource. That could involve a login with a password, or it could involve some other process you specify.
- **Verification:** The server determines that the person should have access. That could involve checking the password against the username, or it could involve another process you specify.
- **Tokens:** The server communicates with the authentication device, like a ring, key, phone, or similar device. After verification, the server issues a token and passes it to the user.
- **Storage:** The token sits within the user's browser while work continues.

If the user attempts to visit a different part of the server, the token communicates with the server again. Access is granted or denied based on the token.

![[Pasted image 20240423232106.png]]
