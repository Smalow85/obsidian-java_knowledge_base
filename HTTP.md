HTTP is a TCP/IP-based application layer communication protocol that standardizes how clients and servers communicate with each other.

## HTTP/0.9 - The One Liner (1991)

It was the simplest protocol ever; having a single method called GET.

```
GET /index.html
```

Response:

```
(response body)
(connection closed)
```

- No headers
- `GET` was the only allowed method
- Response had to be HTML

## HTTP/1.0 - 1996

HTTP/1.0 could now deal with other response formats i.e. images, video files, plain text or any other content type as well. It added more methods (i.e. POST and HEAD), request/response formats got changed, HTTP headers got added to both the request and responses, status codes were added to identify the response, character set support was introduced, multi-part types, authorization, caching, content encoding and more was included.

Here is how a sample HTTP/1.0 request and response might have looked like:

```
GET / HTTP/1.0
Host: cs.fyi
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_10_5)
Accept: */*
```

Response:

```
HTTP/1.0 200 OK 
Content-Type: text/plain
Content-Length: 137582
Expires: Thu, 05 Dec 1997 16:00:00 GMT
Last-Modified: Wed, 5 August 1996 15:55:28 GMT
Server: Apache 0.84

(response body)
(connection closed)
```

One of the major drawbacks of HTTP/1.0 were you couldn’t have multiple requests per connection. That is, whenever a client will need something from the server, it will have to open a new TCP connection and after that single request has been fulfilled, connection will be closed.

To open TCP connection, you must perform the following procedure:

##### Three-way Handshake

- SYN - Client picks up a random number, let’s say x, and sends it to the server.
- SYN ACK - Server acknowledges the request by sending an ACK packet back to the client which is made up of a random number, let’s say y picked up by server and the number x+1 where x is the number that was sent by the client
- ACK - Client increments the number y received from the server and sends an ACK packet back with the number y+1

Once the three-way handshake is completed, the data sharing between the client and server may begin.

![[Pasted image 20240316224014.png]]

## HTTP/1.1 - 1999

- New [[HTTP methods]] were added, which introduced PUT, PATCH, OPTIONS, DELETE
- Hostname Identification In HTTP/1.0 Host header wasn’t required but HTTP/1.1 made it required.
- Persistent Connections. HTTP/1.1 introduced the persistent connections i.e. connections weren’t closed by default and were kept open which allowed multiple sequential requests. To close the connections, the header Connection: close had to be available on the request. Clients usually send this header in the last request to safely close the connection.
- Pipelining It also introduced the support for pipelining, where the client could send multiple requests to the server without waiting for the response from server on the same connection and server had to send the response in the same sequence in which requests were received.
- Chunked Transfers In case of dynamic content, when the server cannot really find out the Content-Length when the transmission starts, it may start sending the content in pieces (chunk by chunk).
- Unlike HTTP/1.0 which had Basic authentication only, HTTP/1.1 included digest and proxy authentication
- Caching
- Byte Ranges
- Character sets
- Language negotiation
- Client cookies
- Enhanced compression support
- New status codes
- ..and more
## HTTP/2 - 2015

HTTP/2 was designed for low latency transport of content. The key features or differences from the old version of HTTP/1.1 include

###### Binary instead of Textual
HTTP/2 tends to address the issue of increased latency that existed in HTTP/1.x by making it a binary protocol. Being a binary protocol, it easier to parse but unlike HTTP/1.x it is no longer readable by the human eye.

HTTP messages are now composed of one or more frames. There is a HEADERS frame for the meta data and DATA frame for the payload and there exist several other types of frames (HEADERS, DATA, RST_STREAM, SETTINGS, PRIORITY etc.)

Every HTTP/2 request and response is given a unique stream ID and it is divided into frames. Frames are nothing but binary pieces of data. A collection of frames is called a Stream.
###### Multiplexing - Multiple asynchronous HTTP requests over a single connection
Once a TCP connection is opened, all the streams are sent asynchronously through the same connection without opening any additional connections. And in turn, the server responds in the same asynchronous way i.e. the response has no order and the client uses the assigned stream id to identify the stream to which a specific packet belongs. This also solves the head-of-line blocking issue that existed in HTTP/1.x i.e. the client will not have to wait for the request that is taking time and other requests will still be getting processed.
###### Header compression using HPACK
When we are constantly accessing the server from a same client there is alot of redundant data that we are sending in the headers over and over, and sometimes there might be cookies increasing the headers size which results in bandwidth usage and increased latency. To overcome this, HTTP/2 introduced header compression.
###### Server Push - Multiple responses for single request
Server push allows the server to decrease the roundtrips by pushing the data that it knows that client is going to demand. How it is done is, server sends a special frame called PUSH_PROMISE notifying the client that, “Hey, I am about to send this resource to you! Do not ask me for it.”
###### Request Prioritization
A client can assign a priority to a stream by including the prioritization information in the HEADERS frame by which a stream is opened. At any other time, client can send a PRIORITY frame to change the priority of a stream.
###### Security
There was extensive discussion on whether security (through TLS) should be made mandatory for HTTP/2 or not. In the end, it was decided not to make it mandatory. However, most vendors stated that they will only support HTTP/2 when it is used over TLS. So, although HTTP/2 doesn’t require encryption by specs but it has kind of become mandatory by default anyway.

## HTTP/3
An important difference in HTTP/3 is that it runs on QUIC, a new transport protocol.  The use of QUIC means that HTTP/3 relies on the User Datagram Protocol ([[UDP]]), not the Transmission Control Protocol ([[TCP]]). Switching to UDP will enable faster connections and faster user experience when browsing online.

QUIC will help fix some of HTTP/2's biggest shortcomings:

Developing a workaround for the sluggish performance when a smartphone switches from WiFi to cellular data (such as when leaving the house or office)
Decreasing the effects of packet loss — when one packet of information does not make it to its destination, it will no longer block all streams of information (a problem known as “head-of-line blocking”)

Other benefits include:

- Faster connection establishment: QUIC allows [[SSL and TLS]] version negotiation to happen at the same time as the cryptographic and transport handshakes
- Zero round-trip time (0-RTT): For servers they have already connected to, clients can skip the handshake requirement (the process of acknowledging and verifying each other to determine how they will communicate)
- More comprehensive encryption: QUIC’s new approach to handshakes will provide encryption by default — a huge upgrade from HTTP/2 — and will help mitigate the risk of attacks

