**REST is an architecture style to develop web services, which uses the HTTP protocol as a communication interface in order to transfer data through HTTP methods.**

![[Pasted image 20240423133831.png]]

REST was born and created in 2000 by Roy Fielding in his PhD dissertation and, according to him:

> _“The name “Representational State Transfer” is intended to evoke an image of how a well-designed Web application behaves: a network of web pages (a virtual state-machine), where the user progresses through the application by selecting links (state transitions), resulting in the next page (representing the next state of the application) being transferred to the user and rendered for their use.”_

### REST principles

The REST architecture has six constraints and an API that is driven by that can be called RESTful:
#### 1. Client-Server

The main principle of the Client-Server web architecture is the Separation of Concerns, which means that the **Client that sends the request it’s completely independent from the Server that returns the response**.
[![Client-Server Constraint](https://res.cloudinary.com/practicaldev/image/fetch/s--b3rBV9h2--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/dbjyai40l64rfj2yowhp.jpg)](https://res.cloudinary.com/practicaldev/image/fetch/s--b3rBV9h2--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/dbjyai40l64rfj2yowhp.jpg)
#### 2. Stateless

All the information (state) that is required in a request must be sended by the Client. Therefore, the Server must not store any data during a Client-Server communication, which means that **every request is a standalone request**.

#### 3. Cache

Cache is a computational storage structure focused on keeping stored data that is frequently accessed, improving performance and network efficiency. Therefore, **through caching, it’s possible to reduce or even eliminate the need for the Client to send requests to the Server (who must inform if the request can be cacheable or not)**.

#### 4. Interface Uniform

Means how Client and Server will share information by defining an interface that must be followed in every request. In other words, **it’s a contract between the Client and the Server that determines the standards for their communication**.

Here, we have four additional constraints that is part of Uniform Interface:
##### 4.1 Identification of Resources

REST is based on resources, and a resource is an information that can be named. **It’s used in a request to identify what the Client wants to access in the Server**.

For example, to retrieve a list of `products`, the resource must be set in the URL: `http://api.example.com/products`
##### 4.2 Manipulation of Resources Through Representation

**The Client must be sure that the request to the Server has enough information to manipulate (create, retrieve, update, delete) the informed resource, which can be represented by multiple formats, such as JSON, XML, HTML etc**. In other words, the Client can specify the desired representation of a resource in every request to a Server.

For example: a Client can specify in a request to retrieve a resource in JSON format.
##### 4.3 Self-descriptive Messages

A self-descriptive message **ensures a uniform interface in the communication by containing all the information that a Client or a Server needs** to understand the request and the response just by checking the semantics of the message.
##### 4.4 HATEOAS (Hypertext As The Engine Of Application State)

HATEOAS means that a **response sent from the Server should contain information about what the Client can do in further requests.** In other words, the Server indicates what actions the Client can do next. In REST standards, Servers must send only hypermedia (links) to Clients.
#### 5. Layered system

Layered system **relates to the fact that there can be more components and subsystems between a Client and a Server.** In other words, the client can’t assume that it is communicating directly to the Server, and don’t know about the complexity to process the request and return the response.

For example: a Client sends a request to a Server, but first it passes by a proxy layer for security check.
[![Layered System Constraint](https://res.cloudinary.com/practicaldev/image/fetch/s--JcyAxveS--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/04xp96134kt5cncsmqum.jpg)](https://res.cloudinary.com/practicaldev/image/fetch/s--JcyAxveS--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/04xp96134kt5cncsmqum.jpg)
#### 6. Code On Demand

Code On Demand is the only optional constraint, and **means that a Server can send an executable code as a response to the Client**. In other words, it’s what happens when a browser, for example, receives a response from the Server with a HTML tag `<script>` so, when the HTML document is loaded, the script can be executed.

## Making requests

REST requires that a client make a request to the server in order to retrieve or modify data on the server. A request generally consists of:

- an [[HTTP methods]], which defines what kind of operation to perform
- a _header_, which allows the client to pass along information about the request
- a path to a resource
- an optional message body containing data

#### Headers and Accept parameters

In the header of the request, the client sends the type of content that it is able to receive from the server. This is called the `Accept` field, and it ensures that the server does not send data that cannot be understood or processed by the client.

For example, a text file containing HTML would be specified with the type `text/html`. If this text file contained CSS instead, it would be specified as `text/css`. A generic text file would be denoted as `text/plain`. This default value, `text/plain`, is not a catch-all, however. If a client is expecting `text/css` and receives `text/plain`, it will not be able to recognize the content.

Other types and commonly used subtypes:

- `image` — `image/png`, `image/jpeg`, `image/gif`
- `audio` — `audio/wav`, `audio/mpeg`
- `video` — `video/mp4`, `video/ogg`
- `application` — `application/json`, `application/pdf`, `application/xml`, `application/octet-stream`
