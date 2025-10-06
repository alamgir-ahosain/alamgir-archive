
<h1 id="top">@RequestMapping</h1>

<h3>📑Table of Contents</h3>

   * [1. Path-Only Mapping (URL-only)](#p)
   * [2. Method-Specific Mapping (Path + HTTP Method)](#m)
   * [3. Base URL Mapping](#b)
   * [4. Shorthand (Method-Specific) Mappings](#s)
   * [5. Consumes and Produces](#c)
   * [6. @RequestBody](#r)
   * [7. @ResponseBody](#re)
   * [8. Content Negotiation & Conversion (produces/consumes)](#co)

---




<h1>@RequestMapping</h1>

- Handles web requests for one or more HTTP methods(GET, POST, PUT, DELETE).
- Can be applied at both class and method levels.

Syntax  
```java
@RequestMapping(
    value = "/path",               // or path = "/path"
    method = RequestMethod.GET,    // HTTP method(s)
    params = "id=10",              // restrict by query parameters
    headers = "key=value",         // restrict by request headers
    consumes = "application/json", // accepted Content-Type
    produces = "application/json"  // response Content-Type
)
```

<h1 id="p">1. Path-Only Mapping (URL-only)</h1>

if don’t specify the method attribute, it handles all HTTP method .
```java
@RequestMapping(path = "/user")
```

Handles :
```java
GET      localhost:8080/user
POST     localhost:8080/user 
PUT      localhost:8080/user 
DELETE   localhost:8080/user 
```




<h1 id="m">2. Method-Specific Mapping (Path + HTTP Method)</h1>
<h3>Case 2.1: Same Path & Same Method</h3> 

```java
@RequestMapping(path = "/user", method = RequestMethod.GET)
@RequestMapping(path = "/user", method = RequestMethod.GET)
```
What happens?
Spring throws an "Ambiguous mapping" error at startup because it cannot decide which method to use.
```java
java.lang.IllegalStateException: Ambiguous mapping. Cannot map 'controllerName' method 
```

<h3>Case 2.2: Same Path, Different Methods</h3>

```java
@RequestMapping(path = "/user", method = RequestMethod.GET)
@RequestMapping(path = "/user", method = RequestMethod.POST)
```
What happens?
This is valid and commonly used.Spring will map:
```java
GET     localhost:8080/user
POST    localhost:8080/user 
```

<h3>Case 2.3: Multiple Methods in One Mapping</h3>

```java
@RequestMapping(path = "/user", method = {RequestMethod.GET,RequestMethod.PUT})
```
Handles both GET and PUT for /user.

[↑ Back to top](#top)

<h1 id="b">3. Base URL Mapping</h1>

every method inside starts with /user.
```java
@RestController
@RequestMapping("/user")
public class UserController{
    
    @GetMapping("/profile")  public String getProfile() { }
    @GetMapping("/order") public String getOrder() { }
}
```
Results:
```java
GET   localhost:8080/user/profile   
GET   localhost:8080/user/order  
```


<h1 id="s">4. Shorthand (Method-Specific) Mappings</h1>

```java
@GetMapping("/path")                         //Handles GET requests only
@PostMapping("/path")                        //Handles POST requests only
@PutMapping("/path")                         //Handles PUT requests only
@DeleteMapping("/path")                     //Handles DELETE requests only
@DeleteMapping(path={"/path","/pathtwo"})   //Handles DELETE requests only
```
<h1 id="c">5. Consumes and Produces  </h1>

<h3>5.1. consumes</h3>

- Defines what Content-Type the API can accept (request body format).
- Matches `client’s Content-Type header`.
- Example: consumes = "application/json" → accepts only JSON input.
- Mismatch → `415 Unsupported Media Type`.

<h3>5.2. produces</h3>

- Defines what data format the API returns.
- Matches `client’s Accept header`.
- Example: produces = "application/xml" → returns XML response.
- Mismatch → `406 Not Acceptable`.

```java
    @RequestMapping(
    path = "/user", 
    method = RequestMethod.GET,
    consumes = MediaType.APPLICATION_JSON_VALUE
    )

    @RequestMapping(
    path = "/user", 
    method = RequestMethod.POST,
    produces = MediaType.APPLICATION_JSON_VALUE
    )
```

> produces : defines response type (Accept header).
>consumes : defines request type (Content-Type header).

[↑ Back to top](#top)

<h1 id="r">6. @RequestBody</h1>

Used to read the HTTP request body into a Java object (POJO).
- Matches with the consumes attribute.
- Incoming request body from a client.
- work as method parameter level specific to POJO class aligned to request body.

```java
@PostMapping("/user")
public String createUser(@RequestBody User user) { ... } 
// Here ,User class is a POJO class (not entity or any type) 
```

<h1 id="re">7. @ResponceBody</h1>

- Outgoing responce body to a client.
- work as method level.
- Matches with the `produces attribute`.
- return type/ value converted to respective data format(JSON/XML) as per configuration produces attribute.


[↑ Back to top](#top)

<h1 id="co">8. Content Negotiation & Conversion (produces/consumes)</h1>

<h3>Case 8.1 – Based on produces:</h3>

```ini
produces = "application/json" → JSON converter → JSON response
produces = "application/xml"  → XML converter  → XML response
```
Each produces type determines which message converter is used for the response.

<h3>Case 8.2 – Multiple Formats</h3>

```ini
produces = {"application/xml", "application/json"}
```

| Client `Accept` Header | Converter Used           | Response Format |
| ---------------------- | ------------------------ | --------------- |
| `application/xml`      | XML converter            | XML response    |
| `application/json`     | JSON converter           | JSON response   |
| `*/*` (any)            | JSON converter (default) | JSON response   |

Spring checks the client’s Accept header and chooses the correct converter.
Defaults to JSON if not specified.

<h3>Case 8.3 – Single Format</h3>

```ini
produces = "application/json"
```

| Client `Accept` Header | Supported? | Response           |
| ---------------------- | ---------- | ------------------ |
| `application/xml`      |   No       | 406 Not Acceptable |
| `application/json`     |   Yes      | JSON response      |
| `*/*`                  |   Yes      | JSON response      |

Only JSON responses are allowed.

<h3>Case 8.4 – Based on consumes:</h3>

```ini
consumes = "application/xml"
```

| Client `Content-Type` | Supported? | Action                          |
| --------------------- | ---------- | --------------------------------|
| XML                   |  Yes       | Request accepted → XML converter|
| JSON                  |  No        | 415 Unsupported Media Type      |


The consumes type specifies which input format the API accepts.




<br>

[↑ Back to top](#top)   <br><br>

