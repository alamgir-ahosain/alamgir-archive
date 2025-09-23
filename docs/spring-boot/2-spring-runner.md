
<h1 id="top"> Spring Boot Runners</h1>

<h3>📑Table of Contents</h3>

1. [Command-Line Arguments](#command)
2. [CommandLineRunner and ApplicationRunner](#com)
3. [Runner class](#run)
4. [Difference between CommandLineRunner and ApplicationRunner](#dif)
---

<h1 id="command">1. Command-Line Arguments</h1>

- To take argument from command line or passing values to main method and access them using 
    - `CommandLineRunner`
    - `ApplicationRunner`
    - `SpringApplication.getRunArguments()`
    - or even with `@Value` injection
- Example 
    `main(String[]args)` : passing values to main method via command line arguments.


<h1 id="com">2. CommandLineRunner and ApplicationRunner </h1>

- executed immediately after SpringApplication.run() completes.
- Having abstract method.

Example:
```java
@SpringBootApplication
public static void main(String[] args) {
    SpringApplication.run(MyApplication.class, args);
    // Runners execute AFTER this
}
```
[↑ Back to top](#top)
<h1 id="run">3. Runner class</h1>

> A component class that implements CommandLineRunner or ApplicationRunner and is registered as a Spring bean is called a Spring Boot runner class.

**Why is it called a Spring Boot runner class?**
beacuse such a class : 
- Runs automatically at startup.
- Is a Spring-managed bean.
- Implements `CommandLineRunner` or `ApplicationRunner` to define startup logic


**Key Points**
- impementing abstract method .
- Executed only once and run automatically.
- Executed logic which write inside implemented of Runner interface .
- used to define logic for loading configuration values, properties.

**Spring Boot application startup lifecycle**

1. Spring Boot App 
2. Start 
3. Run method
    - IoC Container 
    - Bean Object created
    - Runners
    - Started (Application is ready to use. perform operation)



**How many Runners can we add in Spring Boot Application?**   
Answer: Can add unlimited runner classes.


**Multiple Runner class Execution Order**
1. Execute randomly.
2. Control execution order :using  `@Order`
   > least number : higher priority

Example
```java
@Order(1) //executed  second
@Component
public class FirstRunner implements CommandLineRunner {
    @Override
    public void run(String... args) throws Exception {
        System.out.println("FirstRunner ");
    }
}

@Order(-5) //executed  first
@Component
public class SecondRunner implements ApplicationRunner {
    @Override
    public void run(ApplicationArguments args) throws Exception {
        System.out.println("SecondRunner");
    }
}

@Order(2) //executed  third
@Component
public class ThirdRunner implements CommandLineRunner {
    @Override
    public void run(String... args) throws Exception {
        System.out.println("ThirdRunner ");
    }
}

```   


<h1 id="dif">Difference between CommandLineRunner and ApplicationRunner?</h1>

1. **CommandLineRunner**
    - Method : run(String... args)
    - Argument Type : String of Array
    - alamgirPassasArguemts -> main(String[] args)-> SpringApplication.run(MyApplication.class, args)->run(String... args)
    - passing arguemt share the all Runners.

2. **ApplicationRunner**
    - Method : run(ApplicationArguments args)
    - Argument Type : Object (ApplicationArguments)
    - alamgirPassasArguemts -> main(String[] args)-> SpringApplication.run(MyApplication.class, args)-> run(ApplicationArguments args)<br>
    
[↑ Back to top](#top)   
    <br><br>



   
>**Github Code** : [Spring Runner : Spring Boot ](https://github.com/alamgir-ahosain/Learn-Spring-Boot/tree/main/c-runner-class/src/main/java/com/alamgir/c_runner_class)
   
    




 




    