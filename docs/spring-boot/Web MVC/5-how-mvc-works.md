

<h1 id="top">How Spring MVC Works Internally?</h1>




<h3>Front Controller Design Pattern</h3>

>Spring MVC follows the Front Controller design pattern.
- A Front Controller is a single controller that handles all incoming requests and sends back responses.
- In Spring MVC, the DispatcherServlet acts as the Front Controller.



<h3>Step 1: Request to DispatcherServlet</h3>

- First, when a user sends a request, it always comes to the `DispatcherServlet`.
- Example: `GET http://localhost:8080/home`


<h3>Step 2: Handler Mappings</h3>

At application startup, Spring scans all @Controller classes and builds a Handler Mapping of all @RequestMapping methods.
Example:

```java
OrderController : path="/order", GET : create()
UserController  : path="/user",  GET : signup()
```
When a request comes in, Spring checks in the Handler Mappings:
- If a match is not found : 404 Not Found
- If a match is found : The respective controller method is executed


<h3>Step 3: Request Handling</h3>

- The Handler (Controller method) processes the request.
- It may return:
    - Data (JSON/XML) → in REST APIs (@ResponseBody)
    - Logical view name (for web apps with JSP/Thymeleaf)

<h3>Step 4: View Resolver</h3>

- If the controller returns a view name, the `ViewResolver` maps it to an actual view file.

- Controller returns "index" (a logical view name).
- Spring adds the prefix + suffix: `/WEB-INF/view/ + index + .jsp` → `/WEB-INF/view/index.jsp`
- Then, the view layer (JSP/Thymeleaf/HTML) is rendered and returned to the client.


<br>


**Summary Flow**
- Request → DispatcherServlet (Front Controller)
- DispatcherServlet → Handler Mapping (finds matching controller method)
- Controller Method executes (returns data or view name)
- DispatcherServlet → ViewResolver (resolves actual view)
- Response sent back to client   


<br>

[↑ Back to top](#top)   <br><br>
