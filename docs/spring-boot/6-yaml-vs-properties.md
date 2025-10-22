
<h1 id="top"> Config Files: .properties vs .yml, Application vs Bootstrap</h1>

<h3>📑Table of Contents</h3>

1. [.properties vs .yml (YAML)](#p)
2. [application.properties / application.yml](#a)
3. [bootstrap.properties / bootstrap.yml](#b)
---


<h2 id="p">1.  .properties vs .yml (YAML)</h2>

| Feature            | `.properties`                  | `.yml` / `.yaml`                     |
| ------------------ | ------------------------------ | ------------------------------------ |
| Format        | Key-value pairs (`key=value`)  | Hierarchical (uses indentation):`key: value`     |
| Preferred for  | Simple configs                 | Complex, nested configs              |
| Structure  | Sequential, non-nested  | Supports nested/hierarchical data  |
| Repetition  | Keys often repeated	 | Reuses parent keys (no repetition)   |
| Multi-profile  |Needs separate files for each profile| Multiple profiles in one file |





<h3 >Priority between .properties and .yml </h3>
 
 If both `application.properties` and `application.yml` exist in the same location,Spring Boot gives priority to `application.properties`

 application.yml

  ```yml
server:
    port: 8081
  ```
application.properties

```properties
server.port=8088
```
 Result: Port = 8088 (because `.properties` overrides `.yml`)



<h2 id="a"> 2. application.properties / application.yml</h2>

* Default configuration file for Spring Boot.
* Used for:

  * App-specific configs (DB, server, security)
  * Profiles (`application-dev.yml`, etc.)
* Location: `src/main/resources/`


<h2 id="b">3. bootstrap.properties / bootstrap.yml</h2>

* Used for:

  * External config sources (e.g., Spring Cloud Config)
  * Service discovery or environment setup
* Needed only when using **Spring Cloud / Config Server**.
* Location: `src/main/resources/`


<h3>Loading Order</h3>

| File                                       | Loaded | Priority |
| ------------------------------------------ | ------ | -------- |
| `bootstrap.yml / bootstrap.properties`     | First  | Lower    |
| `application.yml / application.properties` | After  | Higher   |



<h3> Example</h3>

bootstrap.yml

```yaml
spring:
  application:
    name: my-service
  cloud:
    config:
      uri: http://localhost:8888
```

application.properties

```properties
server.port=8081
spring.datasource.url=jdbc:mysql://localhost:3306/test
```
application.yml

```yaml
server:
  port: 8081
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/test
```


[↑ Back to top](#top) 

