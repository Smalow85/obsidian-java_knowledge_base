We can use _@RequestHeader_ annotation to read headers individually as well as all together.

If we need access to a specific header, we can configure _@RequestHeader_ with the header name:

```java
@GetMapping("/greeting")
public ResponseEntity<String> greeting(@RequestHeader(HttpHeaders.ACCEPT_LANGUAGE) String language) {
    // code that uses the language variable
    return new ResponseEntity<String>(greeting, HttpStatus.OK);
}
```

Then we can access the value using the variable passed into our method. If a header named _accept-language_ isn’t found in the request, the method returns a “400 Bad Request” error.

If we’re not sure which headers will be present, or we need more of them than we want in our method’s signature, we can use the _@RequestHeader_ annotation without a specific name.

We have a few choices for our variable type: a _Map_, a _MultiValueMap_, or a _HttpHeaders_ object.

```java
@GetMapping("/listHeaders")
public ResponseEntity<String> listAllHeaders(
  @RequestHeader Map<String, String> headers) {
    headers.forEach((key, value) -> {
        LOG.info(String.format("Header '%s' = %s", key, value));
    });

    return new ResponseEntity<String>(
      String.format("Listed %d headers", headers.size()), HttpStatus.OK);
}
```

```java
@GetMapping("/multiValue")
public ResponseEntity<String> multiValue(
  @RequestHeader MultiValueMap<String, String> headers) {
    headers.forEach((key, value) -> {
        LOG.info(String.format(
          "Header '%s' = %s", key, value.stream().collect(Collectors.joining("|"))));
    });
        
    return new ResponseEntity<String>(
      String.format("Listed %d headers", headers.size()), HttpStatus.OK);
}
```

```java
@GetMapping("/getBaseUrl")
public ResponseEntity<String> getBaseUrl(@RequestHeader HttpHeaders headers) {
    InetSocketAddress host = headers.getHost();
    String url = "http://" + host.getHostName() + ":" + host.getPort();
    return new ResponseEntity<String>(String.format("Base URL = %s", url), HttpStatus.OK);
}
```



