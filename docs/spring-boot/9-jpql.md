<h1 id="top">JPQL (Java/Jakarta Persistence Query Language)</h1>

- Avoiding SQL inside Java logic.
- Object-oriented query language for JPA.
- Works with entities and their fields, not database tables/columns.
- **Portable across databases.**
- Converted to SQL by JPA/Hibernate before executing in the database.

<h3>Rules for creating JPQL</h3>

1. select all
   ```sql
   SQL : SELECT * FROM USER
   JPQL: SELECT u FROM User u 
   ```

2. select specific field 
    ```sql
    SQL : SELECT id,name FROM USER
    JPQL: SELECT u.id ,u.name FROM User
    ```

Here(JPQL perspective)
>User = Entity class name (not table name).
>id, name = Entity property names (not column names).

<br>

<h3>Example</h3>

Repository 
```java
public interface UserRepository extends JpaRepository<User, Integer> {
    @Query(value = "SELECT u FROM User u ", nativeQuery = false)
    List<User> getUserJpql();
}
```
Method 
```java
 public void getUserJpqlMethod() {
     List<User> users = userRepository.getUserJpql();
      users.forEach(u -> {
      System.out.println(u.getId() + " " + u.getName() + " " + u.getEmail());
     });
 }
```
SQL 
```sql
JPQL: SELECT u FROM User u 
SQL : SELECT * FROM USER
Hibernate: select u1_0.uid,u1_0.email,u1_0.name from user u1_0
```

<br>

[↑ Back to top](#top)   <br><br>
>**Github Code** : [CRUD Operation](https://github.com/alamgir-ahosain/Learn-Spring-Boot/tree/main/e-crud-opeation) 
