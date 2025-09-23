
<h1 id="top">Dependency Injection (DI)</h1>

<h3>What is Dependency Injection (DI)?</h3>

>A design pattern where an object’s dependencies are provided (injected)by an external source rather than the object creating them itself.

Example of dependencies:
- A `car` depends on an engine and tires.
- A `smartphone` depends on a battery and a screen.

<h3>How is it related to IoC?</h3>

>DI is the implementation of IoC in Spring; the IoC container injects dependencies into beans automatically.

Example
```java
Class A{
    method1()
    method2()
}
class B{
    A a; //non-static variable
    a.method1()
}
```
**What happend?**

- Class A : Contains `method1()` and `method2()`.
- Class B : Contains a non-static variable `A a` and calls `a.method1().`
- Spring Framework : Receives information about A and B, identifies that B depends on A, creates an instance of A internally, and injects it into B.

<h3>Types of Dependency Injection</h3>
            
- Constructor Injection: provided via the class constructor.
- Setter/property Injection: provided via setter methods.
- Field Injection: injected directly into fields using annotations (`@Autowired`).

                 


**The IoC Container in Spring handles mainly three types of dependencies**
- Primitive values (int, float, String, etc.)
- Collection types (List, Set, Map, etc.)
- Reference types (objects of other beans/classes)
<br>

[↑ Back to top](#top)
