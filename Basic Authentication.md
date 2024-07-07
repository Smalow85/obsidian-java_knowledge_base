Basic authentication is a very simple authentication scheme that is built into the HTTP protocol. The client sends HTTP requests with the Authorization header that contains the `Basic` word followed by a space and a base64-encoded `username:password` string.

Upon receiving the request, the server will decode and verify the credentials. If the credentials are valid, the server will send the response to the client.
