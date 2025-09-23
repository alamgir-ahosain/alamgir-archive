
<h1 id="top">Spring Bean Life Cycle</h1>

<h3>📑 Table of Contents</h3>

1. [What is Spring Bean Lifecycle?](#what)
2. [Life Cycle Method](#life)
3. [Ways to Define Lifecycle Methods](#ways)
   - Using xml
   - Using Interface
   - Using Annotation
4. [Question](#question)
---

<h1 id="what">1. What is  Spring Bean Lifecycle?</h1>

- Lifecycle of an individual bean managed by the container.
- key annotation : `@PostConstruct`, `@PreDestroy`, `InitializingBean`, etc.
- Includes : Instantiation, Dependency Injection, Init & destroy callbacks.

<h1 id="life">2. Life Cycle Method</h1>

   1. Instantiation – Spring creates the bean object.
   2. Populate properties / Dependency Injection – Spring injects values or references.
   3. BeanNameAware / BeanFactoryAware callbacks – Optional callbacks if implemented.
   4. Pre-initialization – `@PostConstruct` or` afterPropertiesSet()` (from InitializingBean).
   5. Custom init method – Optional method defined by `init-method`.
   6. Bean is ready to use – Application uses the bean.
   7. Destruction – Container shuts down; `@PreDestroy`, `destroy()` (from `DisposableBean`), or `destroy-method` is called.         


[↑ Back to top](#top)
<h1 id="ways">3. Ways to Define Lifecycle Methods</h1>

**1. Using xml**

   - Spring provide two method to every bean .
   - Good for 3rd-party or external classes.
   - we can change the name of these method but signature must be same
     - `public void init()`: Called after creation via config.Initialization code loading config,connecting db,webservice .
     - `public void distroy()`:For clean up code

 Example:    
```java
<bean id="student" class="Student" init-method="init" destroy-method="cleanup"/>
 public class Student {

     public void init() {
       System.out. println("Bean is initialized");
     }
     public void cleanup() {
        System.out.println("Bean is destroyed");
     }

  }
```

**2. Using Interface**
   - `afterPropertiesSet()` : called after dependencies are injected.
   - `destroy()` : called when container shuts down.

Example:
```java
@Component
 public class Student implements InitializingBean, DisposableBean {
          
            @Override
            public void afterPropertiesSet() throws Exception {
               System.out.println("Bean is initialized");
            }

            @Override
            public void destroy() throws Exception {
               System.out.println("Bean is destroyed");
       }
    }
```   


**3. Using Annotation**
   - Recommended modern approach
   - `@PostConstruct` : Called after bean is created & dependencies injected
   - `@PreDestroy` : called before bean is removed from container.

Example:
```java
@Component
public class Student {
         public Student(){}
         @PostConstruct
         public void init() {
            System.out.println("Bean is initialized");
         }

         @PreDestroy
         public void cleanup() {
            System.out.println("Bean is destroyed");
         }

         public void init2(){
            System.out.println("Bean is initialized again");
         }

         public void cleanup2() {
            System.out.println("Bean is destroyed again");
     }
 }


public class AppConfig{
   @Bean(initMethod = "init2", destroyMethod = "cleanup2")
   public Student student2(){
      return new Student();
   }
}
```

All 4 methods will be called, in this order:
1. Initialization Order (on startup):
   - `@PostConstruct` : `init()`
   - `@Bean(initMethod = "...")` : `init2()`

2. Destruction Order (on shutdown):
      - `@PreDestroy` : `cleanup()`
      - `@Bean(destroyMethod = "...")` : `cleanup2()`


[↑ Back to top](#top)

<h1 id="question">4. Question</h1>

**1. Is it possible to execute logic when a bean object created?**
YES,Ways to Execute Logic After Bean Creation:
   - `init-method `in XML or `@Bean`
   - `@PostConstruct` using annotation
   - `InitializingBean.afterPropertiesSet()` using interface


**2. Difference Between singleton and prototype scope (Bean Lifecycle Perspective)**

1. Singleton
   - Created once at container startup
   - `@PreDestroy` / `destroy()` : Called by Spring when context is closed
   - Bean Destruction Responsibility : Spring container

2. Prototype
   - Created every time the bean is requested.
   - `@PreDestroy` / `destroy() `: Not called automatically
   - Bean Destruction Responsibility : must be handle manually.


<br>

[↑ Back to top](#top)<br><br><br>
>**Github Code** : [Bean Life Cycle : Spring ](https://github.com/alamgir-ahosain/Learn-Spring-Framework/tree/main/d_bean_lifecycle_annotations)
