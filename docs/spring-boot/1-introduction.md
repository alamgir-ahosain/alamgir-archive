<h1>Spring Boot</h1>
<h3>📑Table of Contents</h3>

1. [Why Spring Boot (over Spring) ?](#)
2. [@SpringBootApplication](#)
3. [starter](#)
4. [application.properties](#)

---
<h1 id"">Why Spring Boot (over Spring) ?</h1>

- Auto-config : less boilerplate, faster dev
- Embedded server : Tomcat/Jetty/Undertow, no external WAR deploy
- Actuator : monitoring & health checks
- Defaults : simpler setup (convention over configuration)
- Microservices-ready : easier future scaling
- Starter dependencies : simplifies build config
- No XML/code generation : annotation & Java-based config


<h1 id="">  @SpringBootApplication </h1> 

`@SpringBootApplication` is a convenience annotation that combines three annotations :

- `1. @SpringBootConfiguration` – Marks the class as a configuration class (like @Configuration).
- `2. @EnableAutoConfiguration` – Enables Spring Boot’s auto-configuration feature.
 - `3. @ComponentScan` – Tells Spring to scan the package (and sub-packages) for beans, components, controllers, services, etc.


**Key Points**
- Must be placed on the main application class.
- Scans the package where it’s located and all sub-packages.
- Enables Spring Boot auto-configuration, saving manual setup.    

<h1 id="">starter</h1>

- Collection of Multiple Dependencies.
- Reduces boilerplate configuration — don’t need to figure out which versions of libraries work together.
- Auto-configuration : works with Spring Boot’s auto-config to set up defaults.
- Less version conflicts : Spring Boot manages dependency versions.
- Default Starter : spring-boot-starter


**Example:** 
The Spring Boot Web Starter is:

```pom
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

What it includes (behind the scenes):
- Spring MVC : for REST and web 
- Jackson : for JSON handling
- Validation API (hibernate-validator) : for request validation
- Tomcat (embedded server, default)

So by adding just this one starter, don’t need to manually add each of those dependencies one by one.




<h1 id="">application.properties</h1>

- Always configuring data which is commonly being used by all spring Beans.
- Called as Configuration file.
- key value pair format(key-val).
- two types of properties
    - pre-defiend(`spring.application.name`)
    - used defiend(`db.name=alamgir`)
        
