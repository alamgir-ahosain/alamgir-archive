
<h1 id="top">@RequestBody Validation</h1>

- Used with @Valid or @Validated to validate incoming request body (POJO) before method execution.
- If validation fails, throws a `MethodArgumentNotValidException` and returns a 400 Bad Request response.

**Requirements:**
- Add `spring-boot-starter-validation` dependency (Hibernate Validator).
- Add validation annotations in the model ( @NotNull,@NotEmpty,@Email).
- Use @Valid or @Validated before the @RequestBody parameter.

Example

```java
@PostMapping("/user")
public void createUser(@Valid @RequestBody User user) {}
```

Model Class : 
```java
public class User {
    @NotBlank
    private String name;

    @Email
    private String email;

    @Min(18)
    private int age;
}
```

In Invalid request: 
```json
{
  "name": "",
  "email": "invalid",
  "age": 15
}
```

Response: 400 Bad Request
With error details like:
```json
{
  "errors": [
    "name must not be blank",
    "email must be a valid email address",
    "age must be greater than or equal to 18"
  ]
}
```


<br>

[↑ Back to top](#top)   <br><br>
