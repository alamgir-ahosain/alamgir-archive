
<h1 id="top">Stereotype Annotations</h1>

- Special annotations that tell Spring which layer a class belongs to.
- When Spring starts, it scans packages (`via @ComponentScan`) and registers the class as a Spring Bean in the `ApplicationContext`.
- Can be injected with @Autowired / constructor injection.

> Main Stereotype Annotations
>1. @Component
>2. @Controller
>3. @Service
>4. @Repository

<h3>1. @Component</h3>

- Marks class as a Spring-managed bean.
- Must create Default Constructor otherwise throws `UnsatisfiedDependencyException` .
- Bean object created with deafult constructor execution.
- Can't configure value in bean object(always use default values for properties).
- Used as Class level.

<h3>2. @Controller</h3>

- @Controller tells Spring that this class contains web request handler methods.
- Marks a class as a Spring MVC controller.
- Handles web requests (GET/POST).
- Returns a view name(resolved by ViewResolver) instead of raw data.
- **Used for web pages (MVC), not REST APIs**.
- specialized forms of @Component.



<h3>3. @Service</h3>

>@Service = business logic bean, managed by Spring, used to separate concerns in MVC.
- Contains business logic.
- Supports AOP features (@Transactional).
- specialized forms of @Component.


<h3>4. @Repository</h3>

>@Repository marks a class as a data access component (DAO) in the persistence layer.
- Indicates that the class interacts with the database (CRUD operations, queries).
- Can be used with Spring Data JPA for repository interfaces, though optional there.
- specialized forms of @Component.


<br>

[↑ Back to top](#top)   <br><br>
