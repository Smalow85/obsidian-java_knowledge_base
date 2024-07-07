## DELETE Method

The DELETE method is used to request the server to delete a file at a location specified by the given URL. The following example requests the server to delete the given file **hello.htm** at the root of the server:

`DELETE /hello.htm HTTP/1.1`
`User-Agent: Mozilla/4.0 (compatible; MSIE5.01; Windows NT)`
`Host: www.tutorialspoint.com`
`Accept-Language: en-us`
`Connection: Keep-Alive`

The server will delete the mentioned file **hello.htm** and will send the following response back to the client:

`HTTP/1.1 200 OK`
`Date: Mon, 27 Jul 2009 12:28:53 GMT`
`Server: Apache/2.2.14 (Win32)`
`Content-type: text/html`
`Content-length: 30`
`Connection: Closed`

`<html>`
`<body>`
`<h1>URL deleted.</h1>`
`</body>`
`</html>`