

<h1 id="top">Bean Creation : Annotation-Based Configuration </h1>

<h3>📑Table of Contents</h3>

1. [ @Configuration and @Bean](#con)
2. [@Component and @ComponentScan](#com)
3. [Question](#q)

---
<h4>Creating Bean in two ways:</h4>

1. `@Configuration` and `@Bean`
2. `@ComponentScan` and `@Component`

<h1 id="con">1. @Configuration and @Bean</h1>

- ``@Configuration`` – Marks a class as a source for bean definitions.
- ``@Bean`` 
    - Marks a method’s that return object should be managed by the IoC container.
    - Used at Method level.
    - Return type should be Bean class type.
    - Can configure value in bean object(otherwise use default values for properties). 
         - `@Bean`(name = {"bean1", "alias1", "alias2"})

**How It Works:**
1. Spring scans the ``@Configuration`` class.
2. Methods annotated with ``@Bean`` are called.
3. Return objects are registered as Spring beans.
4. Dependencies can be wired by calling other `@Bean` methods or using `@Autowired`.

Example
```java
    public class Address{
            private String email;
            public void Address(){}
            public void setEmail(String email){this.email=email;}
    }
    
    public class Student{
            private String name;
            public void Student(){}
            public void setName(String name){this.name=name;}
    }

    @Configuration
    public class AppConfig{
            @Bean("student1") 
            public Student getStudent1(){return new Student();} 

            @Bean
            public Student getStudent2(){return new Student();} 
            
            @Bean("address1")
            public Student getAddress(){
                Address address=new Address;
                a.setEmail("a@gmail.com");
                return address;
            }
    }


    class Test{
            ApplicationContext container=AnnotationConfigApplicationContext(AppConfig.class); //create container object
            Student s1=(Student)container.getBean("student1");      //get the bean object
            Student s2=(Student)container.getBean("getStudent2");  //get the bean object
            Address a1=(Address)container.getBean("address1");     //get the bean object
    }
```    
**what happend:**
1. Scans the `@Configuration` class.
2. Methods annotated with `@Bean` are called(`getStudent1`,`getStudent2`,`getAddress`).
3. Return objects(Address) are registered as Spring beans.
4. spring container configure with bean id : `student1`,`getStudent2`,`address1`

[↑ Back to top](#top)
<h3>Rules for @Configuration and  @Bean</h3>

1. In Configuration class If a method is not annotated with `@Bean`, Spring will not execute it for bean creation.
```java
       @Configuration
        public class AppConfig{
            //not a bean. a Plain object ; not registered in Spring context
            public Student getStudent(){return new Student();}  
        }
```
2. If `@Bean` has no explicit ID:
    - Method is executed and Bean object created.
    - **Method name becomes the bean ID**.
```java
        @Configuration
        public class AppConfig{
            @Bean //Bean id is getStudent
            public Student getStudent(){return new Student();}  
        }
```    
3. If duplicate bean IDs exist: throw `BeanDefinitionOverrideException`
```java
        @Configuration
        public class AppConfig{
            @Bean("student")
            public Student getStudent(){return new Student();}  
            @Bean("student")
            public Student getStudent(){return new Student();}  
        }
```    

4. Multiple Configuration class:

```java
    @Configuration
        public class AppConfig1{
            @Bean("student1") 
            public Student getStudent1(){return new Student();} 
            @Bean("address1")public Student getAddress(return new Address){ }
        }
    
    @Configuration
        public class AppConfig2{
            @Bean("student2") 
            public Student getStudent1(){return new Student();} 
            @Bean("address2")
            public Student getAddress(return new Address){ }
        }

    class Test{
            ApplicationContext container=AnnotationConfigApplicationContext(AppConfig1.class,AppConfig2.class); //create container object
            Student s1=(Student)container.getBean("student1");      //get the bean object
            Address a1=(Address)container.getBean("address1");      //get the bean object

            Student s1=(Student)container.getBean("student2");     //get the bean object
            Address a1=(Address)container.getBean("address2");     //get the bean object
        }    
  ```  
[↑ Back to top](#top)
<h1 id="com">2. @Component and @ComponentScan</h1>

**`@Component`**
- Generic stereotype annotation(auto-detect and register beans).
- Marks class as a Spring-managed bean.
- Works with component scanning (<context:component-scan> / `@Component`Scan).
- **Must create Default Constructor** otherwise throws `UnsatisfiedDependencyException` .
- Bean object created with deafult constructor execution.
- **Can't configure value in bean object**(always use default values for properties). 
- **Used as Class level**.


**For `@Component` classes**
- Only parameterized constructor :need default constructor or use `@Autowired` on the parameterized one.
- No constructor :  default constructor created automatically (Spring uses it).
- Both constructors : Spring uses default unless use `@Autowired` on the parameterized one.

**`@ComponentScan`**
- Tells Spring where to scan for components (classes with `@Component`,` @Service`, etc.)
- Scan the Package and Sub-Package .

Example

```java
    package com.spring;
    @Component
    public class AddressClass{}

    package com.spring.pkg;
    @Component("myStudent")
    // `@Component`(value="myStudent")
    public class Student{}


    @Configuration
    @ComponentScan("com.spring") // uses 'value' alias
    @ComponentScan(basePackages = "com.spring")  // uses base Packages explicitly
    @ComponentScan(basePackages = {"com.spring", "com.demo"}) // multiple packages
    @ComponentScan(basePackageClasses = Student.class) // scan package where Student is
    class AppConfig{

    }


    class Test{
        ApplicationContext container = new AnnotationConfigApplicationContext(AppConfig.class);
        Address a=(Address)container.getBean("addressClass");   
        Student s = container.getBean("myStudent", Student.class);
    }

```

**what happend:**
1. spring loads AppConfig(beacuse of `@Configuration`).
2. scans the **package** and **sub-package** using `@ComponentScan`.
3. Finds classes annotated with ``@Component``(Student,AddressClass).
4. Creates instances of Student and AddressClass.
5. Bean ID assignment:
    - for AddressClass = `addressClass` ; beacuse bean id is not specified ,**class name taken as bean id (camel case)**
    - for Student = `myStudent`
     



<h1 id="q">3. Question</h1>

**How can we create a Spring bean for a userdefiend and predefined (third-party) class? Should we use `@Component`, `@Bean`, or both?**
    
- user-Defiend Class : can use either `@Component` or `@Bean`.
- Pre-Defined class : only `@Bean`  beacuse cannot add `@Component` (cannot modify the source code to add annotations)

    ```java

    @Configuration
    public class AppConfig{
        @Bean
        public Date getDate(){return new Date();}
    }
    ```


**Key Points**
> 1. `@Component`: one bean per class (unless  change the scope).
> 2. `@Bean` : can define multiple beans of the same class (different IDs).

<br>

[↑ Back to top](#top)<br><br><br>
>**Github Code** : [Creating Bean: Spring Boot](https://github.com/alamgir-ahosain/Learn-Spring-Boot/tree/main/a-bean-creation)
