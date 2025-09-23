
<h1 id="top">Spring IoC Container(Design Principle)</h1>

<h3>📑Table of Contents</h3>

1. [IoC Container](#ioc)
2. [Types of IoC Container](#type)
3. [Types of ApplicationContext subclasses](#app)
4. [IoC Container Lifecycle](#life)


---
<h1 id="ioc">1. IoC Container</h1>

>IoC(Inversion of Control) : Let the framework control object creation and wiring instead of the application itself.

**Responsible for:**                           
- Bean Creation: Responsible for instantiating, configuring, and assembling beans.
- Bean Lifecycle Management: Manages the lifecycle of bean(objects).
- Dependency Injection: Injecting dependencies(bean) into other beans. 

**Key Points:**
- Enable loosely coupled applications,
    where objects do not need to know about how their dependencies are created.    
- Implements `IoC` through `DI` to provide dependencies at runtime.
- primarily represented by the `ApplicationContext`.


<h1 id="type">2. Types of IoC Container</h1>

1. `BeanFactory`
    - An interface type.
    - Lazily loads beans (creates them only when requested).
    - Provides basic dependency injection.
 2. `ApplicationContext`
    - An interface type.
    - Extends BeanFactory with more features.
    - Supports Internationalization (i18n),Event propagation,Bean post-processing,Access to resources (files, URLs).

[↑ Back to top](#top)

<h1 id="app">3. Types of ApplicationContext subclasses</h1>

1. `ClassPathXmlApplicationContext` – Load Context from XML in classpath.
2. `FileSystemXmlApplicationContext` – XML from file system.
3. `AnnotationConfigApplicationContext` – Java-based config using annotation(`@Configuration`).
4. `WebApplicationContext` – For web apps integrating with Spring MVC.
5. `GenericApplicationContext` – Flexible,can combine XML ,annotation and programmatic Configuration.

**Responsible for:**
- Loads `Config.xml` 
- Reads bean definitions
- Creates & initializes beans inside Spring container
            
Example  
```java
 ApplicationContext container= new ClassPathXmlApplicationContext("Config.xml");
 ```
**What actually happens:**
- Reads Config.xml.
- Finds bean definitions (classes you declared with <bean> or via annotations like @Component).
    - Duplicate bean id : Exception (duplicate bean IDs not allowed).
    - Requesting undefined bean id : Exception (bean not found in config).
    - (Reference of other bean)passing id of other class : bean is matching but data type is not matching. 
- For each bean definition
    - Checks if the class exists in the classpath.
    - If it exists, Spring creates an object (bean).
    - If it’s missing, it throws an error at startup. 

so Spring only checks & creates objects for the configured beans, not for every class in our project.




 
 <h1 id="life">4. IoC Container Lifecycle</h1>

- Lifecycle of the Spring container itself (e.g. `ApplicationContext`).
- Scope is Global : affects the entire application context.
- Includes : Context creation,Event publishing,Bean loading,Shutdown handling.
- Key annotations/APIs:`ApplicationListener`, `ContextRefreshedEvent`, etc.
- Managed by : Spring framework at the container level.

**Life Cycle Method** 

1. Initialization: Container starts, reads configuration, and loads bean definitions.
2. Bean Instantiation: Creates bean objects using constructors or factory methods.
3. Dependency Injection (DI): Injects required dependencies into beans.
4. Lifecycle Management: Calls `@PostConstruct` methods after initialization and `@PreDestroy` before destruction.
5. Singleton Scope:** By default, one instance per bean is maintained in the container.
6. ApplicationContext Events: Publishes events that components can listen to for loosely coupled, event-driven design.
7. Bean Post-Processing: BeanPostProcessors can modify beans before and after initialization.

<br>

[↑ Back to top](#top)