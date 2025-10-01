<h1 id="top">Monolithic Architecture: Frontend and Backend in the Same Layer</h1>

<h3>1. Add View Layer</h3>

Inside your src/main folder, create the following structure:

```css
src/main/webapp
 └── WEB-INF
      └── view
           └── index.jsp
```

<h3>2. Configure View Resolver</h3>

In `application.properties`, tell Spring where to look for JSP files:

```properties
spring.mvc.view.prefix=/WEB-INF/view/
spring.mvc.view.suffix=.jsp
```

So, if a controller returns "index", Spring looks for /WEB-INF/view/index.jsp.

<h3>3. Implement MVC Architecture</h3>

- Controller : handles request
- View : JSP page for UI
- Model : data passed between controller and view


<h3>4. Add Tomcat JSP Support</h3>

- By default, embedded Tomcat in Spring Boot does not support JSP.
  
  
so add dependency ,also need JSTL (Java Standard Tag Library):

 ```xml
    <!-- https://mvnrepository.com/artifact/org.apache.tomcat.embed/tomcat-embed-jasper -->
    <dependency>
        <groupId>org.apache.tomcat.embed</groupId>
        <artifactId>tomcat-embed-jasper</artifactId>
        <version>11.0.7</version>
    </dependency>

    <!-- JSTL Support -->
    <dependency>
        <groupId>jakarta.servlet</groupId>
        <artifactId>jakarta.servlet-api</artifactId>
        <scope>provided</scope>
    </dependency>

 ```




<br>

[↑ Back to top](#top)   <br><br>

