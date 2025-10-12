<h1 id="top">HTTP Headers</h1>


- Used to share **metadata** between client and server.  
- Sent as part of **HTTP request and response**.  
- Represented as **key–value pairs**.  
- Can include **custom headers** (e.g., `X-Auth-Token`).  

<h1> Types of Headers</h1>

| Type | Description |
|------|--------------|
| **General Headers** | Apply to both requests and responses. |
| **Request Headers** | Sent by client to provide information about the request. |
| **Response Headers** | Sent by server to provide information about the response. | 
| **Entity Headers** | Describe the body content (if present). |

---

<h1>Common Request Headers</h1>

| Header | Description | Example |
|---------|--------------|----------|
| `Host` | Domain name of the server. | `Host: example.com` |
| `User-Agent` | Info about client software. | `User-Agent: Chrome/141.0` |
| `Accept` | Expected response format. | `Accept: application/json` |
| `Authorization` | Credentials for authentication. | `Authorization: Bearer <token>` |
| `Cookie` | Sends stored cookies. | `Cookie: sessionId=abc123` |



<h1> Common Response Headers</h1>

| Header | Description | Example |
|---------|--------------|----------|
| `Content-Type` | Media type of the response body. | `Content-Type: application/json` |
| `Content-Length` | Size of response body in bytes. | `Content-Length: 348` |
| `Server` | Server software info. | `Server: Apache/2.4.56` |
| `Set-Cookie` | Sends cookies to client. | `Set-Cookie: sessionId=abc123` |
| `Location` | Used in redirects. | `Location: /login` |
| `Cache-Control` | Defines caching rules. | `Cache-Control: no-cache` |

---


<h3>Request Headers </h3>
Example

```
GET /users HTTP/1.1
Host: api.example.com
Accept: application/json
Authorization: Bearer xyz123
```

<h3>Response Headers</h3>

Example

```
HTTP/1.1 200 OK
Content-Type: application/json
Cache-Control: no-cache
```

[↑ Back to top](#top) 

<h3> 1. Read Header Value from Request</h3>

> To read header values from an incoming request, two common ways:

- Using RequestHeader
    - Reads a specific header directly.
    - Use` @RequestHeader Map<String, String>` to get all headers.
- Using HttpServletRequest
    - `request.getHeader("name")` : read a single header.
    - `request.getHeaderNames()` : get all header names.



<h2>@RequestHeader – Required and Default Value Behavior</h2>

```java

@RequestHeader(
    name="Security-token" ,
    required=false,
    defaultValue = "token"
    ) String token

```




| Client Sends Header? | `required` Value   | `defaultValue` Provided? | Result              | Description                               |
| -------------------- | ------------------ | ------------------------ | ------------------- | ----------------------------------------- |
|  Yes                | —                  | —                        |  Value from client | The header value sent by client is used.  |
|  No                 | `required = true`  | —                        |  400 Bad Request   | Header is mandatory, but missing.         |
|  No                 | `required = false` | —                        |  Value = `null`    | Header is optional; value will be `null`. |
|  No                 | `required = false` |  Yes                     |  Value = token     | Default value used instead of `null`.     |


<h1>2. Add Header to Response</h1>

Using ResponseEntity

```java
@GetMapping("/custom-header")
public ResponseEntity<String> sendHeader() {
    return ResponseEntity
            .ok()
            .header("X-App-Name", "SpringBootDemo")
            .body("Header added successfully!");
}
```
Here,
- header("name", "value") → Adds custom header to response.
- add multiple headers: .headers(HttpHeaders headers)



[↑ Back to top](#top) 
