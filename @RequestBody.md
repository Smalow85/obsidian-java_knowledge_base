`@RequestBody` annotation maps the `HttpRequest` body to a transfer or domain object, enabling automatic deserialization of the inbound `HttpRequest` body onto a Java object.

```java
@PostMapping("/request")
public ResponseEntity postController(
  @RequestBody LoginForm loginForm) {
 
    exampleService.fakeAuthenticate(loginForm);
    return ResponseEntity.ok(HttpStatus.OK);
}
```

Spring automatically deserializes the JSON into a Java type, assuming an appropriate one is specified.

By default, the type we annotate with the _@RequestBody_ annotation must correspond to the JSON sent from our client-side controller.
