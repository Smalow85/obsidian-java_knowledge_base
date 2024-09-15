`@RequestMapping` can be applied to the controller class as well as methods.

**@RequestMapping with Class:** We can use it with class definition to create the base URI. For example:
  ```java
    @Controller
    @RequestMapping("/home")
    public class HomeController {
    
    }
    ```

Now /home is the URI for which this controller will be used. This concept is very similar to servlet context of a web application.

**@RequestMapping with Method**: We can use it with method to provide the URI pattern for which handler method will be used. For example:

```java
@RequestMapping(value="/method0")
@ResponseBody
public String method0(){
    return "method0";
}
```
  
**@RequestMapping with Multiple URI**: We can use a single method for handling multiple URIs, for example:

```java
@RequestMapping(value={"/method1","/method1/second"})
@ResponseBody
public String method1(){
    return "method1";
}
```

If you will look at the source code of **RequestMapping annotation**, you will see that all of it’s variables are arrays. We can create String array for the URI mappings for the handler method.
    
**@RequestMapping with HTTP Method**: Sometimes we want to perform different operations based on the HTTP method used, even though request URI remains same. We can use [@RequestMapping](https://www.digitalocean.com/community/users/requestmapping) method variable to narrow down the HTTP methods for which this method will be invoked. For example:

```java
@RequestMapping(value="/method2", method=RequestMethod.POST)
@ResponseBody
public String method2(){
    return "method2";
}
   	
@RequestMapping(value="/method3", method={RequestMethod.POST,RequestMethod.GET})
@ResponseBody
public String method3(){
    return "method3";
}
```

**@RequestMapping with Headers**: We can specify the headers that should be present to invoke the handler method. For example:

```java
@RequestMapping(value="/method4", headers="name=pankaj")
@ResponseBody
public String method4(){
    return "method4";
}
	
@RequestMapping(value="/method5", headers={"name=pankaj", "id=1"})
@ResponseBody
public String method5(){
    return "method5";
}
    ```

**@RequestMapping with Produces and Consumes**: We can use header `Content-Type` and `Accept` to find out request contents and what is the mime message it wants in response. For clarity, `@RequestMapping` provides **produces** and **consumes** variables where we can specify the request content-type for which method will be invoked and the response content type. For example:

```java
@RequestMapping(value="/method6", produces={"application/json","application/xml"}, consumes="text/html")
@ResponseBody
public String method6(){
    return "method6";
}
```
    
Above method can consume message only with **Content-Type as text/html** and is able to produce messages of type **application/json** and **application/xml**.