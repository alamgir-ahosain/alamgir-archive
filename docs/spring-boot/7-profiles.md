<h2 id="top">1. Spring Boot Profiles</h2>

>Profiles allow  to define different configurations for different environments
(e.g., dev, test, prod,uat).

<h2>2. Defining Profiles</h2>

create profile-specific files:
- application-dev.properties   : local development
- application-test.properties   : testing/staging
- application-prod.properties : production
- application-uat.properties     : User Acceptence Testing

<h2>3. Way of activating a Profile</h2>

- In application.properties: `spring.profiles.active=dev`
- Command line: `--spring.profiles.active=dev`
- Environment variable: `SPRING_PROFILES_ACTIVE=dev`

<h2>4. Profile File Loading Order</h2>

- `application.properties `: common config (base/default)
- `application-{profile}.properties` : overrides base config when active


<h4>Example</h4>

application.properties
```properties
spring.profiles.active=dev
server.port=8080
```

application-dev.properties
```properties
server.port=8081
```

application-prod.properties
```properties
server.port=8082
```

What happend?
- Active profile: dev 
- Base config: application.properties → server.port=8080
- Profile-specific override: application-dev.properties → server.port=8081
- Result: server.port = 8081

>If active profile = prod → server.port = 8082
>If no profile → server.port = 8080


<h2>5. Use Case</h2>

```java
@Profile(value={"dev"})
@Component
public class User {
    // Bean logic here
}
```

What Happend?

- `@Profile("dev")` : loads the bean only when the active profile matches `"dev"`.
- Bean `User` is created and available in the Spring context.
- If active **profile ≠ "dev"** : Bean User is not created and Spring skips it.


[↑ Back to top](#top) 