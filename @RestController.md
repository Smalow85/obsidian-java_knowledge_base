_@RestController_ is a specialized version of the controller. It includes the [[@Controller]] and [[@ResponseBody]] annotations, and as a result, simplifies the controller implementation:

```java
@RestController
@RequestMapping("books-rest")
public class SimpleBookRestController {
    
    @GetMapping("/{id}", produces = "application/json")
    public Book getBook(@PathVariable int id) {
        return findBookById(id);
    }

    private Book findBookById(int id) {
        // ...
    }
}
```

The controller is annotated with the _@RestController_ annotation; therefore, the _@ResponseBody_ isn’t required.

Every request handling method of the controller class automatically serializes return objects into _HttpResponse_.