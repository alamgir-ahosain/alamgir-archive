<h1 id="top">@JsonProperty</h1>

- Part of Jackson library for JSON serialization/deserialization.
- Maps a Java field/getter/setter to a specific JSON property name.
- Useful when `JSON field names differ from Java field names`.


Example
```java
public class User {
    @JsonProperty("user_name")
    private String userName;

    @JsonProperty("user_email")
    private String email;
}
```

Resulting JSON:

```json
{
  "user_name": "Alamgir",
  "user_email": "alamgir@example.com"
}
```
<br>

[↑ Back to top](#top)   <br><br>
