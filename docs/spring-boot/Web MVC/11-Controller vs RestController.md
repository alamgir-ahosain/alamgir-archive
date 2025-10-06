
<h1 id="top">@Controller vs @RestController</h1>

<h2 >1. @Controller</h2>

- @Controller tells Spring that this class contains web request handler methods.
- Marks a class as a Spring MVC controller.
- Handles web requests (GET/POST).
- Returns a view name(resolved by ViewResolver) instead of raw data.
- Used for web pages (MVC), not REST APIs.
- specialized forms of `@Component`.

```java
@Controller
public class HomeController {

    @RequestMapping(path = "/home", method = RequestMethod.GET)
    public String homePage() {
        // This will be resolved to home.jsp or home.html
        return "home";
    }
}
```

<h2>2. @ResponseBody</h2>

- Tells Spring not to render a view, but instead Sends return value directly to HTTP response.
- Used for JSON, XML, or text.
- Can be applied to methods in a @Controller.

```java
@Controller
public class UserController {

    @RequestMapping(path = "/user", method = RequestMethod.GET)
    @ResponseBody
    public String userMessage() {
        // This will return JSON (if Jackson is on the classpath)
        return "message";
    }
}
```


<h2>3. @RestController</h2>

>A convenience annotation that combines: @Controller + @ResponseBody

- Every method automatically returns data (JSON/XML) instead of view.
- Commonly used for REST APIs.

 ```java
 @RestController
public class ProductController {

     RequestMapping(path = "/product", method = RequestMethod.GET)
    public String productMessage() {
        return "message";
        // No need to use @ResponseBody, handled automatically
    }
}
```

[↑ Back to top](#top)