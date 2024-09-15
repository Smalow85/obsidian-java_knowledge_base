We can use _@RequestParam_ to extract query parameters, form parameters, and even files from the request.
#### Name
We can configure the _@RequestParam_ name using the _name_ attribute:

```java
@PostMapping("/api/foos")
@ResponseBody
public String addFoo(@RequestParam(name = "id") String fooId, @RequestParam String name) { 
    return "ID: " + fooId + " Name: " + name;
}
```
#### Optional or not

We can configure our _@RequestParam_ to be optional, though, with the _required_ attribute:

```java
@GetMapping("/api/foos")
@ResponseBody
public String getFoos(@RequestParam(required = false) String id) { 
    return "ID: " + id;
}
```

#### Default value

We can also set a default value to the _@RequestParam_ by using the _defaultValue_ attribute:

```java
@GetMapping("/api/foos")
@ResponseBody
public String getFoos(@RequestParam(defaultValue = "test") String id) {
    return "ID: " + id;
}
```
#### Mapping All Parameters

We can also have multiple parameters without defining their names or count by just using a _Map_:

```java
@PostMapping("/api/foos")
@ResponseBody
public String updateFoos(@RequestParam Map<String,String> allParams) {
    return "Parameters are " + allParams.entrySet();
}
```