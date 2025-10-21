
<h1 id="top">Exception Handling in REST Services</h1>

<h3>📑Table of Contents</h3>

1. [Exception Handling Overview](#eo)
2. [Controller Advice Class](#c)
3. [Exception Handler Methods](#em)
4. [Default (Catch-All) Exception Handler](#d)
5. [Scope of Exception Handling](#s)
6. [Exception Handling Flow](#ef)

Validation Note
>The validation annotations @NotEmpty and @NotBlank cannot be applied to int or Integer type fields.They are only valid for CharSequence, Collection, Map, or Array types, not numeric types.


<h2 id="eo"> 1. Exception Handling Overview</h2>

In Spring Boot REST APIs, exception handling can be centralized using `@ControllerAdvice` and `@ExceptionHandler`.These annotations separate exception-handling logic from the business logic in controllers.


<h2 id="c">2. Controller Advice Class</h2>

- A class annotated with `@ControllerAdvice`
- Contains methods annotated with `@ExceptionHandler`
- Acts as a centralized/global exception handler for all controllers
- Used to handle exceptions globally as part of a request lifecycle
- Can handle specific exception types via separate `@ExceptionHandler` methods




<h2 id="em">3. Exception Handler Methods</h2>

>An exception handler method is defined inside either a controller or a controller advice class.

**Rules**

- The return type should be `ResponseEntity<?>`
- Inside `@ExceptionHandler,` specify which exception(s) to handle.
- The method can accept:
   - The exception type as a parameter.
   -  Optionally, an `HttpServletRequest` argument for request-specific details.


Example 
```java
@ExceptionHandler(ResourceNotFoundException.class)
public ResponseEntity<String> handleResourceNotFound(
    ResourceNotFoundException ex, HttpServletRequest request) {
    return ResponseEntity.status(HttpStatus.NOT_FOUND).body("Resource not found: " + ex.getMessage());
}
```

[↑ Back to top](#top)

<h2 id="d">4. Default (Catch-All) Exception Handler</h2>

If no specific exception type is defined, a generic handler can be used to catch all unhandled exceptions — acting as a fallback
```java
@ExceptionHandler(Exception.class)
public ResponseEntity<String> handleException(Exception ex) {
    return ResponseEntity.internalServerError().body("An unexpected error occurred. Please try again later.");
}
```

<h2 id="s">5. Scope of Exception Handling</h2>

If  define an `@ExceptionHandler` method 


| Level            | Location                   | Scope            | Description                                 |
| ---------------- | -------------------------- | ---------------- | ------------------------------------------- |
| Controller-level | Inside controller          | Local            | only handles exceptions thrown by that controller.|
| Global-level     | Inside `@ControllerAdvice` | Application-wide | handles exceptions thrown by any controller in the application.      |




<h2 id="ef">6. Exception Handling Flow</h2>

- If an exception occurs inside a controller:
    - Spring first checks if that controller has a local `@ExceptionHandler` method for that exception.
- If not found:
    - Spring then looks for a matching handler in any `@ControllerAdvice` class.
- If still not found:
    - A default Spring error response is returned.



<br>

[↑ Back to top](#top)   <br><br>

>**Github Code** : [Exception Handling ](https://github.com/alamgir-ahosain/Learn-Spring-Boot/tree/main/k-exception-handling)

