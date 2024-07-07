The PUT method is used to request the server to store the included entity-body at a location specified by the given URL. The following example requests the server to save the given entity-body in **hello.htm** at the root of the server:

`PUT /hello.htm HTTP/1.1`
`User-Agent: Mozilla/4.0 (compatible; MSIE5.01; Windows NT)`
`Host: www.tutorialspoint.com`
`Accept-Language: en-us`
`Connection: Keep-Alive`
`Content-type: text/html`
`Content-Length: 182`

`<html>`
`<body>`
`<h1>Hello, World!</h1>`
`</body>`
`</html>`

The server will store the given entity-body in **hello.htm** file and will send the following response back to the client:

`HTTP/1.1 201 Created`
`Date: Mon, 27 Jul 2009 12:28:53 GMT`
`Server: Apache/2.2.14 (Win32)`
`Content-type: text/html`
`Content-length: 30`
`Connection: Closed`

`<html>`
`<body>`
`<h1>The file was created.</h1>`
`</body>`
`</html>`