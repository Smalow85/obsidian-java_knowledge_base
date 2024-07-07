A GET request retrieves data from a web server by specifying parameters in the URL portion of the request. This is the main method used for document retrieval. The following example makes use of GET method to fetch hello.htm:

`GET /hello.htm HTTP/1.1`
`User-Agent: Mozilla/4.0 (compatible; MSIE5.01; Windows NT)`
`Host: www.tutorialspoint.com`
`Accept-Language: en-us`
`Accept-Encoding: gzip, deflate`
`Connection: Keep-Alive`

The server response against the above GET request will be as follows:

`HTTP/1.1 200 OK`
`Date: Mon, 27 Jul 2009 12:28:53 GMT`
`Server: Apache/2.2.14 (Win32)`
`Last-Modified: Wed, 22 Jul 2009 19:15:56 GMT`
`ETag: "34aa387-d-1568eb00"`
`Vary: Authorization,Accept`
`Accept-Ranges: bytes`
`Content-Length: 88`
`Content-Type: text/html`
`Connection: Closed`

`<html>`
`<body>`
`<h1>Hello, World!</h1>`
`</body>`
`</html>`