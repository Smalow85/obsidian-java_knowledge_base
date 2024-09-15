The _@ResponseBody_ annotation tells a controller that the object returned is automatically serialized into JSON and passed back into the _HttpResponse_ object.

```java
@Controller
@RequestMapping("/post")
public class ExamplePostController {

    @Autowired
    ExampleService exampleService;

    @PostMapping("/response")
    @ResponseBody
    public ResponseTransfer postResponseController(
      @RequestBody LoginForm loginForm) {
        return new ResponseTransfer("Thanks For Posting!!!");
     }
}
```

In the developer console of our browser or using a tool like Postman, we can see the following response:

```plaintext
{"text":"Thanks For Posting!!!"}
```

Remember, we don’t need to annotate the [[@RestController]] annotated controllers with the _@ResponseBody_ annotation since Spring does it by default.

When we use the _@ResponseBody_ annotation, we’re still able to explicitly set the content type that our method returns.

For that, we can use the `@RequestMapping` _produces_ attribute. Note that annotations like _@PostMapping_, _@GetMapping_, etc. define aliases for that parameter.

```java
@PostMapping(value = "/content", produces = MediaType.APPLICATION_JSON_VALUE)
@ResponseBody
public ResponseTransfer postResponseJsonContent(
  @RequestBody LoginForm loginForm) {
    return new ResponseTransfer("JSON Content!");
}
```

In the example, we used the _MediaType.APPLICATION_JSON_VALUE_ constant. Alternatively, we can use _application/json_ directly.

