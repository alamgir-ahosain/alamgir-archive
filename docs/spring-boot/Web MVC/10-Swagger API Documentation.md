<h1>Swagger API Documentation</h1>


- Automates REST API documentation in Spring Boot.
- Collects all endpoints and displays them in a UI format.
- To enable add dependency: `Swagger starter` (e.g., springdoc-openapi-starter-webmvc-ui).

    ```xml
    <!-- https://mvnrepository.com/artifact/com.spring4all/swagger-spring-boot-starter -->
    <dependency>
        <groupId>com.spring4all</groupId>
        <artifactId>swagger-spring-boot-starter</artifactId>
        <version>2.0.2</version>
    </dependency>
    ```


- Access Swagger UI:
```bash
http://hostname:port/context-path/swagger-ui.html
```