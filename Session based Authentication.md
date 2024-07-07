Session-based authentication is a **stateful** [[Authentication (API)]] technique where we use sessions to keep track of the authenticated user. Here is how Session Based Authentication works:

1. User submits the login request for authentication.
2. Server validates the credentials. If the credentials are valid, the server initiates a session and stores some information about the client. This information can be stored in memory, file system, or database. The server also generates a unique identifier that it can later use to retrieve this session information from the storage. Server sends this unique session identifier to the client.
3. Client saves the session id in a cookie and this cookie is sent to the server in each request made after the authentication.
4. Server, upon receiving a request, checks if the session id is present in the request and uses this session id to get information about the client.

And that is how session-based authentication works.