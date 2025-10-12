<h1 id="top">HTTP Response Status Codes </h1>




- **3-digit numbers** returned by a server.
- Represent the **execution status or outcome** of an HTTP request.
- Always **provided by the web server or web application** after processing a request.



| Category | Code Range | Meaning | 
|-----------|-------------|----------|
| **1xx** | 100–199 | Informational |
| **2xx** | 200–299 | Success | 
| **3xx** | 300–399 | Redirection |
| **4xx** | 400–499 | Client Error |
| **5xx** | 500–599 | Server Error | 


<h3>Common Codes</h3>

| Code | Meaning | Description |
|------|----------|-------------|
| **100** | Continue | Request received, continue. |
| **101** | Switching Protocols | Protocol change accepted. |
| **102** | Processing | Request in progress. |
| **200** | OK | Request succeeded. |
| **201** | Created | New resource created. |
| **202** | Accepted | Request accepted for processing, but not completed yet. |
| **204** | No Content | Success, no body returned. |
| **301** | Moved Permanently | Resource moved to new URL. |
| **302** | Found | Temporary redirect. |
| **304** | Not Modified | Cached version still valid. |
| **400** | Bad Request | Invalid syntax. |
| **401** | Unauthorized | Authentication needed. |
| **403** | Forbidden | Access denied. |
| **404** | Not Found | Resource doesn’t exist. |
| **405** | Method Not Allowed | HTTP method not supported for the resource. |
| **500** | Internal Server Error | General server error. |
| **503** | Service Unavailable | Server overloaded or down. |



# Difference Between 204 and 404

| Status Code | Meaning | When Used | Response Body |
|--------------|----------|------------|----------------|
| **204 No Content** | Request succeeded, but there’s **no content** to return. | When the server successfully processes a request, but doesn’t need to send any data back (e.g., DELETE success, or empty result set). |  No body |
| **404 Not Found** | The **requested resource doesn’t exist** on the server. | When the client asks for a resource (URL) that’s not found or invalid. |  No body (or optional error message) |



 Example
- **204** → "User deleted successfully, no data to return."  
- **404** → "User not found in the system."

---
[↑ Back to top](#top) 

<h1>ResponseEntity in Spring</h1>

- Represents the **whole HTTP response**.
- Used to **control status code, headers, and body** in a REST API response.
- Belongs to package: `org.springframework.http`.
- Structure : `ResponseEntity<T>`



```java
@RequestMapping(path = "/user/v2", method = RequestMethod.POST, consumes = MediaType.APPLICATION_JSON_VALUE)
    public ResponseEntity<String> createUser2(@RequestBody UserRequest userRequest) {

        String msg = userService.CreateUser(userRequest);

        if (msg.equals("User Created Succesfully..")) {
            // return new ResponseEntity<String>(msg, HttpStatusCode.valueOf(201));
            return ResponseEntity.status(HttpStatus.CREATED).body(msg);
        }
        // return new ResponseEntity<String>(msg, HttpStatusCode.valueOf(200));
        // return ResponseEntity.ok(msg);
        return ResponseEntity.status(HttpStatus.OK).body(msg);
    }
```    





```java
 return ResponseEntity.ok(msg);
 return new ResponseEntity<String>(msg, HttpStatusCode.valueOf(200));

 return ResponseEntity.noContent().build();
 return new ResponseEntity<String>(null, HttpStatusCode.valueOf(204));

```        


[↑ Back to top](#top)   <br><br>
