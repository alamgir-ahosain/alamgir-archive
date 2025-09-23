
<h1 id="top">Inject Primitive and String Type in Spring</h1>

<h3>📑 Table of Contents</h3>

1. [Application Properties](#ap)
2. [Using @Value Annotation](#val)
5. [Using @PropertySource](#prop)

---

<h1 id="ap">1. Application Properties</h1>

- Always configuring data which is commonly being used by all spring Beans.
- Called as Configuration file.
- key value pair format(key-val).
- two types of properties
    - pre-defiend(spring.application.name)
    - used defiend(db.name=alamgir)

Example `application.properties` file:

```properties
spring.application.name=b-dependency-injection
db.port=8080
db.url=localhost:8080
db.password=mysql
db.database=mysql,oracle,postgres
```

[↑ Back to top](#top)
<h1 id="val"> 2. Using @Value Annotation</h1>

##### Injecting values directly:

```java
@Value("8080")
private int port;

@Value("localhost:8080")
private String url;
```

##### Injecting from properties:

```java
@Value("${db.password}")
private String password;
```

##### Default Values
```java
@Value("${db.name:mysql}")
private String name;
```
If the property is missing, the default value `mysql` will be used.

##### Injecting Multiple Values
For list-type properties:

```java
@Value("${db.database}")
private List<String> database;
```

 If `db.database=mysql,oracle,postgres` : it becomes a list: `[mysql, oracle, postgres]`


 **How it works:**

1. Spring checks `application.properties` (or `application.yml`).
2. Looks for the property.
   * If  Found : proceeds.
   * If Not found : throws `java.lang.IllegalArgumentException: Could not resolve placeholder 'db.password'.`
3. Injects the value into the field.

[↑ Back to top](#top)
<h1 id="prop"> 3. Using @PropertySource</h1>

Add external `.properties` files:

```java
@Configuration
@PropertySource("classpath:db.properties")
public class AppConfig { }
```

### Key Points

* `@PropertySource` tells Spring where to load a `.properties` file from.
- Used on  a @Configuration class.
* Works with `@Value("${property.name}")`.
* Can load multiple files:
    ```java
    @Configuration
    @PropertySource("classpath:db.properties")
    public class AppConfig { }  
    ```
    `classpath:` looks inside `src/main/resources`.
* If a property exists in **both** `application.properties` and a `@PropertySource` file:
    - Spring Boot **uses the value from application.properties**.
    - `@PropertySource` has **lower priority** than default Spring Boot property files.<br>

    [↑ Back to top](#top)<br><br><br>
    >**Github Code** : [Inject primitives and String type : Spring Boot](https://github.com/alamgir-ahosain/Learn-Spring-Boot/tree/main/b-dependency-injection/src/main/java/com/alamgir/b_dependency_injection/inject_primitives)



