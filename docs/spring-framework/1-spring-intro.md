
<title>Intro - alamgir</title>
<h3 id="top">📑Table of Contents</h3>

1. [What is Spring?](#spring)
2. [What is Reflection API?](#reflection)
3. [Spring Standalone Collection](#collection)
4. [Transitive dependency in JAVA](#tra)

<h1 id="spring"># What is Spring?</h1>

- Spring is a Dependency Injection framework to make Java applications loosely coupled.
- Makes the development of `JavaEE` applications easier.


<h1 id="reflection">What is Reflection API?</h1>

- Allows inspect and manipulate classes, methods, fields, constructors at runtime.
- Can work without knowing names at compile time.
- Part of package: `java.lang.reflect`.
- Key classes: `Class`, `Method`, `Field`, `Constructor`, `Modifier`.
- Used in frameworks like Spring, Hibernate, JUnit for dynamic behavior.

Example
```java 
@Autowired
private Address address;
```
field injection with `@Autowired` is powered by Reflection API.

>Reflection = Runtime inspection + manipulation of classes and objects.



<h1 id="collection">Spring Standalone Collection</h1>

- A collection (List, Set, Map, or Properties) managed by Spring.
- Injected into a Spring bean via XML or Java configuration.
- Standalone means it is defined directly in Spring, without external frameworks.
- Useful for storing multiple values and injecting them into beans.



<h1 id="tra">Transitive dependency in JAVA</h1>

>A transitive dependency is an indirect dependency included in a project because a direct dependency requires it.

**Direct dependency** : declared explicitly.
**Transitive dependency** : pulled in through another dependency.

1. Maven Example:
    `Project → Library A → Library B`
    Here, Library B is a transitive dependency.
<br>
2. In Spring
    `spring-core → depends on → spring-jcl.`
    if we download spring-core jar file then maven automatically also download spring-jcl jar file. 


<br>

[↑ Back to top](#top)

