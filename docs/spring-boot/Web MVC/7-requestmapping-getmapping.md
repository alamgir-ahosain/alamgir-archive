
<h3>@RequestMapping</h3>

- Can handle all HTTP methods (GET, POST, PUT, DELETE)
- Syntax: `@RequestMapping(value="/path", method=RequestMethod.GET)`

<h3>@GetMapping</h3>

- Handles GET requests only
- Syntax: `@GetMapping("/path")`
- Simpler and cleaner for GET requests

---

Example:

```java
    @Controller
    public class HomeController {

       @RequestMapping(value="/home", method=RequestMethod.GET)
        public String home() { return "index";}

        @GetMapping("/home")
        public String home() {  return "index";}

    }
```

