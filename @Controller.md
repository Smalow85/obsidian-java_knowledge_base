We can annotate classic controllers with the _@Controller_ annotation. This is simply a specialization of the [[@Component]] class, which allows us to auto-detect implementation classes through the classpath scanning.

We typically use _@Controller_ in combination with a _@RequestMapping_ annotation for request handling methods.

```java
@Controller
@RequestMapping("books")
public class SimpleBookController {

    @GetMapping("/{id}", produces = "application/json")
    public @ResponseBody Book getBook(@PathVariable int id) {
        return findBookById(id);
    }

    private Book findBookById(int id) {
        // ...
    }
}
```

We annotated the request handling method with _@ResponseBody_. This annotation enables automatic serialization of the return object into the _HttpResponse_.

