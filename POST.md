The POST method is used when you want to send some data to the server, for example, file update, form data, etc. The following example makes use of POST method to send a form data to the server, which will be processed by a process.cgi and finally a response will be returned:

`POST /cgi-bin/process.cgi HTTP/1.1`
`User-Agent: Mozilla/4.0 (compatible; MSIE5.01; Windows NT)`
`Host: www.tutorialspoint.com`
`Content-Type: text/xml; charset=utf-8`
`Content-Length: 88`
`Accept-Language: en-us`
`Accept-Encoding: gzip, deflate`
`Connection: Keep-Alive`

`<?xml version="1.0" encoding="utf-8"?>`
`<string xmlns="http://clearforest.com/">string</string>`

The server side script process.cgi processes the passed data and sends the following response:

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
`<h1>Request Processed Successfully</h1>`
`</body>`
`</html>`