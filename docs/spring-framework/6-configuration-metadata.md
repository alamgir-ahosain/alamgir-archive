  
  <h1 id="top">Configuration Metadata</h1>
                  

>Where we declare beans and it's dependency.The container knows what to instantiate and configure using configuration metadata .


**Three ways to Configuring Metadata**
1. XML
    - Beans and their dependencies are declared in an XML file (commonly `config.xml` or` applicationContext.xml`).
    - Spring container reads the XML file and creates objects accordingly.

2. Java annotations
    - Done by annotating classes, methods, or fields.
    - Use `@Component`,`@Autowired`
    - `@Autowired` – Most common annotation; tells the IoC container to inject dependencies.

3. Java code
    - `@Configuration` – Marks a class as a source for bean definitions.
    - `@Bean` – Indicates that a method’s return object should be managed by the IoC container.
   

<h1>Spring XML DI Example - Project Procedure</h1>

```java
class Student{
        int id
        String name
       }
```


1. Create Maven Project : Set up project structure using Maven.
2. Add Dependencies : Add **Spring Core** and **Spring Context** in `pom.xml`.
3. Create Bean (`Student.java`) : Plain Old Java Object (POJO) with properties `id` and `name`.
4. Create Configuration File (`config.xml`) :Define the **student1** bean and set properties.
```xml
    <bean id="student1" class="com.core.Student"> </bean>
```

5. Setter Injection : Use setter methods in `Student.java` to inject `id` and `name`.
6. Main Class :
    - Load Spring `ApplicationContext`.
    - Retrieve Student bean and use it.

```java
       ApplicationContext container= new ClassPathXmlApplicationContext("Config.xml");
       Object obj =container.getBean("student1");
       Student s1=(Student)obj;
```




**Key Points:**
 >Bean : An object managed by the IOC container.
 >DI – Container injects dependencies.
 >IoC – DI is Spring’s way of implementing IoC. 

 <br>

 [↑ Back to top](#top)<br><br>
 >**Github Code** : [Configuration Metadata](https://github.com/alamgir-ahosain/Learn-Spring-Framework/tree/main/j_configuration_metadata)