

<h1 id="top">Spring Boot Actuator</h1>



<h3>📑Table of Contents</h3>

1. [Common Actuator Endpoints](#c)
2. [Enable Actuator](#e)
3. [ Actuator Endpoints Security](#s)
4. [Customizing Actuator Endpoints Base Path](#cu)
5. [Custom Actuator Endpoint](#cus)

---

>pring Boot Actuator provides production-ready features to help  monitor and manage  application.

<h3 id="c">1. Common  Actuator Endpoints</h3>

* `/actuator/health` – app health status
* `/actuator/info` – shows app info (can include custom data)
* `/actuator/metrics` – shows performance metrics
* `/actuator/beans` – lists all configured beans
* `/actuator/env` – shows environment properties
* `/actuator/mappings` – lists all request mappings
* `/actuator/shutdown` – gracefully shuts down the app
* `/actuator/threaddump` – shows thread dump

<h3 id="e">2. Enable by adding the dependency</h3>

```xml
<!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-actuator -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
    <version>3.5.6</version>
</dependency>
```

[↑ Back to top](#top) 

<h3 id="s">3. Spring Actuator Endpoints Security </h3>

By default, only `/health` and `/info` are publicly accessible; all other Actuator endpoints require Spring Security configuration.
Configure exposed endpoints in `application.properties`

```properties
# - enable all endpoints
management.endpoints.web.exposure.include=*

# - enable all endpoints except beans
management.endpoints.web.exposure.exclude=beans


# - enable only beans and health
management.endpoints.web.exposure.include=beans,health
```

<h3 id="cu">4. Customizing Actuator End Points Base Path</h3>

By default base-path of actuator endpoints is `/actuator`.We can change the default `/actuator` base path using:

```properties
management.endpoints.web.base-path=/management
```


<h3 id="cus">5. Spring Actuator Custom Endpoints</h3>

can be access through `actuator/myendpoint`

```java
@Endpoint(id = "myendpoint")
@Component
public class CustomActuator {

    @Bean
    @ReadOperation
    public String customActuatorMethod() {
        return "This is Custom Actuator Endpoint";
    }
}
```

[↑ Back to top](#top) 