Single sign-on (SSO) is a technology which combines several different application login screens into one. With SSO, a user only has to enter their login credentials (username, password, etc.) one time on a single page to access all of their [SaaS](https://www.cloudflare.com/learning/cloud/what-is-saas/) applications. SSO is often used in a business context, when user applications are assigned and managed by an internal IT team. Remote workers who use SaaS applications also benefit from using SSO.
## How does an SSO login work?

Whenever a user signs in to an SSO service, the service creates an **authentication token** that remembers that the user is verified. An authentication token is a piece of digital information stored either in the user's browser or within the SSO service's servers, like a temporary ID card issued to the user. Any app the user accesses will check with the SSO service. The SSO service passes the user's authentication token to the app and the user is allowed in. If, however, the user has not yet signed in, they will be prompted to do so through the SSO service.

An SSO service does not necessarily remember who a user is, since it does not store user identities. Most SSO services work by checking user credentials against a separate identity management service.

Think of SSO as a go-between that can confirm whether a user's login credentials match with their identity in the database, without managing the database themselves — somewhat like when a librarian looks up a book on someone else's behalf based on the title of the book. The librarian does not have the entire library card catalog memorized, but they can access it easily.

## How do SSO authentication tokens work?

The ability to pass an authentication token to external apps and services is crucial in the SSO process. This is what enables identity verification to take place separately from other cloud services, making SSO possible.

Think of an exclusive event that only a few people are allowed into. One way to indicate that the guards at the entrance to the event have checked and approved a guest is to stamp each guest's hand. Event staff can check the stamps of every guest to make sure they are allowed to be there. However, not just any stamp will do; event staff will know the exact shape and color of the stamp used by the guards at the entrance.

Just as each stamp has to look the same, authentication tokens have their own communication standards to ensure that they are correct and legitimate. The main authentication token standard is called [[SAML]]. Similar to how webpages are written in HTML (Hypertext Markup Language), authentication tokens are written in SAML.

