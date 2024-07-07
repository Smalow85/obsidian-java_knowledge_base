OAuth (Open Authentication) is an open-standard authorization protocol or framework that provides applications the ability for “secure designated access.”

OAuth doesn’t share password data but instead uses authorization tokens to prove an identity between consumers and service providers. OAuth is an authentication protocol that allows you to approve one application interacting with another on your behalf without giving away your password.
## How OAuth Works

There are 3 main players in an OAuth transaction: the user, the consumer, and the service provider.

![[Pasted image 20240424173216.png]]

**OAuth is built on the following central components:**

##### Scopes and Consent
Scopes are what you see on the authorization screens when an app requests permissions. They’re bundles of permissions asked for by the client when requesting a token. These are coded by the application developer when writing the application.
##### Actors
The actors in OAuth flows are as follows:

- **Resource Owner**: owns the data in the resource server. For example, I’m the Resource Owner of my Facebook profile.
- **Resource Server**: The API which stores data the application wants to access
- **Client**: the application that wants to access your data
- **Authorization Server**: The main engine of OAuth
- 
![[Pasted image 20240424173717.png]]

##### Clients
Clients can be public and confidential. There is a significant distinction between the two in OAuth nomenclature. Confidential clients can be trusted to store a secret. They’re not running on a desktop or distributed through an app store.
##### Tokens
Access tokens are the token the client uses to access the Resource Server (API). They’re meant to be short-lived. Because these tokens can be short lived and scale out, they can’t be revoked, you just have to wait for them to time out.
The other token is the refresh token. This is much longer-lived; days, months, years. This can be used to get new tokens. To get a refresh token, applications typically require confidential clients with authentication. Refresh tokens can be revoked.
You can use the access token to get access to APIs. Once it expires, you’ll have to go back to the token endpoint with the refresh token to get a new access token.
##### Authorization Server

Tokens are retrieved from endpoints on the authorization server. The two main endpoints are the authorize endpoint and the token endpoint. They’re separated for different use cases. The authorize endpoint is where you go to get consent and authorization from the user.
The two main endpoints are the authorize endpoint and the token endpoint. They’re separated for different use cases. The authorize endpoint is where you go to get consent and authorization from the user. This returns an authorization grant that says the user has consented to it. Then the authorization is passed to the token endpoint. The token endpoint processes the grant and says “great, here’s your refresh token and your access token”.
##### Flows

 **Implicit Flow** - all the communication is happening through the browser. There is no backend server redeeming the authorization grant for an access token.
 **Authorization Code Flow** uses both the front channel and the back channel. The front channel flow is used by the client application to obtain an authorization code grant. The back channel is used by the client application to exchange the authorization code grant for an access token (and optionally a refresh token).
 **Client Credential Flow** - It’s a back channel only flow to obtain an access token using the client’s credentials. It supports shared secrets or assertions as client credentials signed with either symmetric or asymmetric keys.
**Resource Owner Password Flow** is very similar to the direct authentication with username and password scenario and is not recommended. In this flow, you send the client application a username and password and it returns an access token from the Authorization Server.
**Assertion Flow** is similar to the client credential flow. This flow allows an Authorization Server to trust authorization grants from third parties such as SAML IdP.
**Device Flow** - There’s no web browser, just a controller for something like a TV. A user code is returned from an authorization request that must be redeemed by visiting a URL on a device with a browser to authorize.
