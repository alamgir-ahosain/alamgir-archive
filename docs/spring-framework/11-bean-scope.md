
<h1 id="top">Spring Bean Scopes </h1>

- In Spring Framework, bean scopes define the lifecycle and visibility of beans in the Spring IoC container.
- `@Scope` defines a bean's lifecycle and visibility of the bean.

<h2>Common @Scope Values</h2>

1. **singleton( by default)**
   - Only one instance of the bean per Spring container.
   - Same object is shared everywhere.
   - This is the default scope if no other scope is specified.
   - `Lifecycle`: The bean is created when the application context is initialized and destroyed when the context is destroyed.
   <br>

2. **prototype**
   - A new instance is created every time the bean is requested.9
   - `Lifecycle`: Created every time requested, not managed by Spring after that.
   <br>
3. **request (Web-aware only)**
   - One bean instance per HTTP request.
   - Used in web applications (Spring MVC).
   - `Lifecycle`: Created for each HTTP request, destroyed at request end
   <br>
4. **session(Web-aware only)**
   - One bean instance per HTTP session.
   - `lifecycle`: Created for each HTTP session, destroyed when session ends
 <br>
5. **application (Web-aware only)**
   - One bean instance per ServletContext (entire web app).
   - Similar to singleton but specific to web context.
   - `Lifecycle`: Created when ServletContext is initialized, destroyed at shutdown.
   <br>
6. **websocket(Web-aware only, since Spring 4.2)**
   - One bean instance per WebSocket session.
   - `Lifecycle`: Created when WebSocket session is established, destroyed at session end
 

**Example**:
 1. XML
 ```xml
  <bean id="myBean" class="com.example.MyBean" scope="prototype"/>
 ```   
  
 2. Annotation

   ```java  
    @Component            // stereotype (registers the bean)
    @Scope("prototype")   // modifies its lifecycle(Define the scope as prototype)
    public class MyBean { 
      //Bean defination
   }

   ```


[↑ Back to top](#top)
<h1>Points to remember</h1>

- @Scope is applied at the class level.
- Affects the Spring-managed bean, not the class itself.
- Each bean (even from the same class) can have a different scope.
- Does not apply globally to the class — only to the specific bean definition.

Example
```java
@Configuration
public class AppConfig {

    @Bean
    @Scope("singleton")  // First bean with singleton scope
    public MyBean singletonBean() {
      // This scope applies only to the Spring-managed bean of singletonBean
        return new MyBean();
    }

    @Bean
    @Scope("prototype")  // Second bean with prototype scope
    public MyBean prototypeBean() {
      // This scope applies only to the Spring-managed bean of prototypeBean
        return new MyBean();
    }
}


```

<br>

[↑ Back to top](#top)<br><br><br>
>**Github Code** : [Bean Scope : Spring](https://github.com/alamgir-ahosain/Learn-Spring-Framework/tree/main/h_bean_scope)
