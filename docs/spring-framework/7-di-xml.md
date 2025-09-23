<h1 id="top">Spring Dependency Injection via XML</h1>

<h3>📑Table of Contents</h3>

1. [Setter Injection](#si)
2. [Constructor Injection](#ci)
3. [Bean Wiring](#bw)
4. [Autowiring Modes](#am)
5. [When to Use Setter and Constructor Injection](#when)

---

**Two ways to inject dependencies via XML**
1.  Setter Injection
2. Constructor Injection

<h1 id="si">1. Setter Injection</h1>

- Dependencies are injected via setter methods after the bean is instantiated.<br>
- Declared in XML with `property` tag

### Setter Injection process:
1. Object is created using the default constructor.  
2. Setter methods are called to inject values or references.  
3. Default Constructor and Setter method must be specified.

When Using:  
```xml
<property name="" class=""></property> or 
<property name="" ref="">
````
1. Creates the object(bean).
2. Identify the setter method for the property.
3. Passes the value (or reference) to the setter and dependency is injected.

### Supports:

1. Attribute or p-schema injection
2. Collections (List, Set, Map) injection
3. Reference type (other beans) injection

### Example Code:

```java
Class Address {
    private String email;
    public void setEamil(String email){ this.email=email; }
}

Class Student {
    private String name;
    private Address address;   // Reference type
    List<String> fruits;       // Collection type
      
    public void setId(int id){ this.id=id; }
    public void setName(String name){ this.name=name; }
    public void setAddress(Address address){ this.address = address; }
    public void setSkills(List<String> fruits){ this.fruits = fruits; }
}

```

### Example XML:

```xml
<!-- Attribute or p-schema injection -->
<bean id="addressBean" class="Address" p:email="alamgir.ahosain@gmail.com"/>

<bean id="student1" class="Student">
    <property name="name" value="Alamgir"/>
    <property name="address" ref="addressBean"/> <!-- Reference type injection -->

    <property name="fruits">
        <!-- Collections (List) injection -->
        <list>
            <value>Mango</value>
            <value>Jack Fruit</value>
        </list>
    </property>
</bean>
```

### Scenario:

```java
Class Address{}
Class Home{}
Class Student { 
    private Address address;
    public void setAddress(Address address){ this.address = address; }
}

<bean id="address" class="Address"></bean>
<bean id="homeBean" class="Home"></bean>
<bean id="student1" class="Student">
    <property name="address" ref="homeBean"/>
</bean>
```

**Question:**
**What happens if a bean’s ref points to another bean with a mismatched type?**

Two possible cases:
* Bean not found  : throws `NoSuchBeanDefinitionException`
* Bean type mismatch : throws `BeanCreationException`

---

**Question:**

```java
Class Student{}
```

1. Is it possible to create bean object or not? → **YES**
2. Is it possible to do Setter Injection, Constructor Injection or both? → **NO**

---
[↑ Back to top](#top)
<h1 id="ci">2. Constructor Injection  </h1>

- Constructor Injection provided via the class constructor.
- Object Creation and Injection happen at the same time.
- Constructor must be specified.


### Constructor Injection Process:

1. Object is created via a constructor with arguments (No need setters).
2. Values or bean references are passed directly through the constructor.

**Ambiguity arises when multiple constructors exist with same argument types.<br>**
**Fix using** :
1. index : specify parameter position
2. type : specify parameter type
3. name : specify parameter name

```java
public Student(String name, int id) {   // constructor 1
   this.name = name;
   this.id = id;
}
public Student(int id, String name) {   // constructor 2
   this.id = id;
   this.name = name;
}
```

### Solutions:

```xml
<!-- 1. Use index(parameter position (0, 1, 2 …).) -->
<bean id="student" class="Student">
    <constructor-arg index="0" value="Alamgir"/> <!-- String first -->
    <constructor-arg index="1" value="12"/>   <!-- int second -->
</bean>

<!-- 2. Use type( parameter data type.) -->
<bean id="student" class="Student">
    <constructor-arg type="String" value="Alamgir"/>
    <constructor-arg type="int" value="12"/>
</bean>

<!-- 3. Use name(if compiled with parameter) (specify parameter name) -->
<bean id="student" class="Student">
    <constructor-arg name="name" value="Alamgir"/>
    <constructor-arg name="id" value="12"/>
</bean>
```

---
[↑ Back to top](#top)
<h1 id="bw">3. Bean Wiring </h1>

- Connecting one bean to another bean.
- Done by using the **ref** attribute.

```xml
<bean id="addressBean" class="Address"></bean>
<bean id="student" class="Student">
    <property name="address" ref="addressBean"/>
</bean>
```

---
[↑ Back to top](#top)
<h1 id="am">4. Autowiring Modes</h1>



Allows to automatically inject dependent beans without explicitly using the `ref` attribute.
### Four configuration values for `autowire` attribute:

1. **no** : manual wiring only, no automatic injection.

2. **byType**

   - Matches bean by class type.
   - Uses **setter injection** by default.
   - Constructor injection is not used unless explicitly with `@Autowired`.
   - Setter required, Constructor not used.
   - Bean ID not used.

   **Situtation when:**
   - No bean of the type → throws `NoSuchBeanDefinitionException`.
   - Exactly one bean of the type → injects successfully.
   - More than one bean of the type → throws `NoUniqueBeanDefinitionException`.

3. **byName**

   * Matches property name with bean ID.
   * Type doesn’t matter.
   * Setter required,, Constructor not used.
   * Bean ID is used.

   **Situtation when:**
   - No bean with matching name → throws `NoSuchBeanDefinitionException`.
   - Exactly one bean with matching name → injects successfully.
   - Duplicate bean names → Duplicate bean definition throws `BeanDefinitionStoreException`.

4. **constructor**

   * Matches constructor argument type with bean ID.
   * Works only with constructors.

     **Situtation when:**
   - No bean of the type → `NoSuchBeanDefinitionException`.
   - Exactly one bean of the type → injects successfully.
   - If more than one bean of type exist → tries to match constructor parameter name with bean ID.If found then `injects` otherwise throws `NoUniqueBeanDefinitionException`. 

---
<h1> Example Classes</h1>

```java
class Address {
   private String name;
   public Address() {}
   public Address(String name){ this.name=name; }
   public void setName(String name){ this.name=name; }
} 

class Student {
    private Address addressBean;
    public Student() {}
    public Student(Address addressBean){ this.addressBean=addressBean; }
    public void setAddressBean(Address addressBean){ this.addressBean=addressBean; }
}
```

---

### byType
 ( property name addressBean is type of Address (Address in Student))

```xml
<bean id="aBean" class="Address">
    <property name="name" value="Alamgir"/>
</bean>
<bean id="studentBean" class="Student" autowire="byType"/>
```

---

### byName
(property name addressBean = bean ID addressBean)
```xml
<bean id="addressBean" class="Address">
    <property name="name" value="Alamgir"/>
</bean>
<bean id="studentBean" class="Student" autowire="byName"/>
```

---

### constructor

```xml
<bean id="addressBean" class="Address">
    <property name="name" value="Alamgir"/>
</bean>
<bean id="studentBean" class="Student" autowire="constructor"/>
```

Steps:
- Finds the `Student(Address addressBean)` constructor.
- Looks for a bean of type `Address`.
- Injects it automatically.

<h1 id="when">5. When to Use Setter and Constructor Injection</h1>

**Use Setter Injection when:**

1. Dependencies are optional (object can be created without them).
2. To set partial/optional properties.
3. Properties may change or override later (mutable).
4. Easier config when many properties.

**Use Constructor Injection when:**

1. Dependencies are mandatory (object cannot exist without them).
2. To set all/mandatory properties at once.
3. Object should be immutable after creation.
4. Ensures required dependencies at object creation time.<br>


[↑ Back to top](#top)   <br><br>
   
>**Github Code** : [Dependency Injection Basic : spring ](https://github.com/alamgir-ahosain/Learn-Spring-Framework/tree/main/c_dependency_injection_basic)<br>
>**Github Code** : [Autowiring : spring ](https://github.com/alamgir-ahosain/Learn-Spring-Framework/tree/main/e_autowiring)




   
    

