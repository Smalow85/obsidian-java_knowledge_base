**JWT**, or JSON Web Token, is an open standard used to share security information between two parties — a client and a server. Each JWT contains encoded JSON objects, including a set of claims. JWTs are signed using a cryptographic algorithm to ensure that the claims cannot be altered after the token is issued.

A JSON web token (JWT) is an [open standard](https://tools.ietf.org/html/rfc7519). The finished product allows for safe, secure communication between two parties. Data is verified with a digital signature, and if it's sent via HTTP, encryption keeps the data secure.  

JWTs have three important components.

1. **Header:** Define token type and the signing algorithm involved in this space.
2. **Payload:** Define the token issuer, the expiration of the token, and more in this section.
3. **Signature:** Verify that the message hasn't changed in transit with a secure signature.

*There are many benefits of JWTs.*

- **Size:** Tokens in this code language are tiny, and they can be passed between two entities quite quickly.
- **Ease:** Tokens can be generated from almost anywhere, and they don't need to be verified on your server.
- **Control:** You can specify what someone can access, how long that permission lasts, and what the person can do while logged on.

*There are also potential disadvantages.*

- **Single key:** JWTs rely on a single key. If that key is compromised, the entire system is at risk.
- **Complexity:** These tokens aren’t simple to understand. If a developer doesn’t have a strong knowledge of cryptographic signature algorithms, they could inadvertently put the system at risk.
- **Limitations:** You can’t push messages to all clients, and you can’t manage clients from the server side.

