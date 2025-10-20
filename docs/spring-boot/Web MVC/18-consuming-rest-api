

<h1 id="top">Spring REST Client</h1>

- REST clients in Spring ( RestTemplate, RestClient, WebClient, and OpenFeign) 
- used to consume or call external REST services.  
- Aacting as HTTP clients for communicating with other APIs(send requests and receive responses).




<h2>Requirements for Consuming a REST API</h2>
When consuming an external REST service, you need to know:

- API URL – The endpoint of the target REST API.
- HTTP Method – e.g., GET, POST, PUT, or DELETE.
- Response Type – The Java type (class) expected in the response.
- Request Body – The data to send (if applicable).
- Required Headers – Such as Content-Type, Authorization, etc.


<h2>Handling JSON Responses</h2>

>When the API returns a JSON payload, create a POJO class that matches the response structure.
>Pass this POJO class as the response type in RestTemplate.

Example:

```json
[
    {
    "id": 1,
    "title": "Fjallraven - Foldsack No. 1 Backpack, Fits 15 Laptops",
    "price": 109.95,
    "description": "Your perfect pack for everyday use and walks in the forest."
    },
    .....
]
```

```java
public class ProductDto {
    private int id;
    private String title;
    private double price;
    private String description;
}
```

```java
ResponseEntity<ProductDto> response =
    restTemplate.getForEntity(url, ProductDto.class);
```    

<h3>HttpEntity</h3>

> HttpEntity is used to wrap both the request body and headers together.

```java
HttpHeaders headers = new HttpHeaders();
headers.set("Authorization", "Bearer token");
headers.setContentType(MediaType.APPLICATION_JSON);

HttpEntity<ProductDtoRequest> entity = new HttpEntity<>(requestBody, headers);
ResponseEntity<ProductDto> response =  restTemplate.exchange(url, HttpMethod.POST, entity, ProductDto.class);
```

# Comparison of REST API Clients in Spring Boot
| Client Type  | Synchronous | Asynchronous | Recommended For             | Notes                                       |
|---------------|-------------|--------------|------------------------------|---------------------------------------------|
| RestTemplate  | Yes         | No           | Simple apps                 | Deprecated but maintained                   |
| WebClient     | No          | Yes          | Reactive systems            | Best for scalable, non-blocking use cases   |
| RestClient    | Yes         | Optional     | Modern Spring Boot (3.2+)   | Easiest and preferred API style today       |
| OpenFeign     | Yes         | No           | Microservices architecture  | Declarative client with annotation support  |


---
<br>

[↑ Back to top](#top)   <br><br>

>**Github Code** : [Consuming REST API](https://github.com/alamgir-ahosain/Learn-Spring-Boot/tree/main/i-consuming-rest-api)

