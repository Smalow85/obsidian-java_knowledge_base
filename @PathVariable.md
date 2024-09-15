_@PathVariable_ annotation can be used to handle template variables in the request URI mapping, and set them as method parameters.

```java
@GetMapping("/api/employees/{id}")
@ResponseBody
public String getEmployeesById(@PathVariable String id) {
    return "ID: " + id;
}
```

In the previous example, we skipped defining the name of the template path variable since the names for the method parameter and the path variable were the same.

However, if the path variable name is different, we can specify it in the argument of the _@PathVariable_ annotation:

```java
@GetMapping("/api/employeeswithvariable/{id}")
@ResponseBody
public String getEmployeesByIdWithVariableName(@PathVariable("id") String employeeId) {
    return "ID: " + employeeId;
}
```

Depending on the use case, we can have more than one path variable in our request URI for a controller method, which also has multiple method parameters:

```java
@GetMapping("/api/employees/{id}/{name}")
@ResponseBody
public String getEmployeesByIdAndName(@PathVariable String id, @PathVariable String name) {
    return "ID: " + id + ", name: " + name;
}
```

We can set the _required_ property of _@PathVariable_ to _false_ to make it optional. Thus, modifying our previous example, we can now handle the URI versions with and without the path variable:

```java
@GetMapping(value = { "/api/employeeswithrequiredfalse", "/api/employeeswithrequiredfalse/{id}" })
@ResponseBody
public String getEmployeesByIdWithRequiredFalse(@PathVariable(required = false) String id) {
    if (id != null) {
        return "ID: " + id;
    } else {
        return "ID missing";
    }
}
```

