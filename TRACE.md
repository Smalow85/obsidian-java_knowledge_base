The TRACE method is used to echo the contents of an HTTP Request back to the requester which can be used for debugging purpose at the time of development. The following example shows the usage of TRACE method:

`TRACE / HTTP/1.1`
`Host: www.tutorialspoint.com`
`User-Agent: Mozilla/4.0 (compatible; MSIE5.01; Windows NT)`

The server will send the following message in response to the above request:

`HTTP/1.1 200 OK`
`Date: Mon, 27 Jul 2009 12:28:53 GMT`
`Server: Apache/2.2.14 (Win32)`
`Connection: close`
`Content-Type: message/http`
`Content-Length: 39`

`TRACE / HTTP/1.1`
`Host: www.tutorialspoint.com`
`User-Agent: Mozilla/4.0 (compatible; MSIE5.01; Windows NT)`