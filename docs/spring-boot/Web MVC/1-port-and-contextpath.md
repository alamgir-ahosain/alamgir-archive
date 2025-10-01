<h1 id="top">Configure Port number</h1>

>A port is a virtual endpoint that lets applications and services communicate over a network.

- Default Port: Run on port 8080 by default(accessible at http://localhost:8080/).
- can configure a custom port in application.properties `server.port=8012`
- After this, project  will be accessible at http://localhost:8012/


<h1>Context path</h1>

> Defines the root path of an application.
 
- **In Servlets** 
    - The context path is usually the name of the WAR file deployed.
    - If deploy myproject.war ,this project accessible at http://localhost:8080/myproject
    - Can be changed by modifying the `server.xml` or the deployment descriptor.
 
- **In Spring Boot**
    - By default, Spring Boot run at the root (/) : http://localhost:8080/
    - To set a custom context path, configure it in `application.properties or application.yml`:
    `server.servlet.context-path=/myproject`
    - After this, project  will be accessible at http://localhost:8080/myproject


<br>

[↑ Back to top](#top)   <br><br>
