

<h1 id="top">CRUD operation workflow</h1>

**1. Create Database table.**
```sql
  CREATE TABLE user (
      id INT,
      name VARCHAR(50),
      email VARCHAR(50)
  );
```
**2. Create Entity class**
```java
@Entity(name = "user")
public class User {

    @Id
    @Column(name = "uid")
    private int id;

    @Column
    private String name;

    @Column
    private String email;
    
    //Getters and Setters
}
```
**3. Create JPA Repository**
```java

public interface UserRepository extends JpaRepository<User,Integer> {} 

```
**4. Perform Operation**
- Create a Bean class
- Inject repository into bean class.
- Repository having pre-defiend method to do DB operations.

```java
@Component
public class Crud {

    @Autowired
    private UserRepository userRepository;

    public void insertUser() {
        User u = new User();
        u.setId(12);
        u.setName("Alamgir");
        u.setEmail("alamgir.ahosain@gmail.com");
        userRepository.save(u);
    }
}
```
**5. Call the method**
```java
@SpringBootApplication
public class ECrudOpeationApplication {

	public static void main(String[] args) {
		ConfigurableApplicationContext context = SpringApplication.run(ECrudOpeationApplication.class, args);
		Crud c = context.getBean(Crud.class);
		c.insertUser();
	}
}
```
<br>

[↑ Back to top](#top)   <br><br>
