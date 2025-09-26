<h1 id="top">CRUD Operation</h1>

<h3>📑Table of Contents</h3>

  1. [Question and Concepts](#q) 
     - @Transactional
     - save()
     - Why Hibernate runs SELECT before INSERT?
     - Custom / Dervied findBy method
  5. [Insert](#i)
  6. [Updare](#u)
  7. [Read](#r)
  8. [Delete](#d)
---
<h1 id="q">1. Question and Concepts</h1>
<h2>1.1 @Transactional with Hibernate</h2>

- opens a transaction before method execution and commits/rolls back after.
- Executes Hibernate operations (INSERT/UPDATE/DELETE).
- Without @Transactional : insert may not execute (no commit), or may hit `LazyInitializationException`.

<h2>1.2 save()</h2>

> save() does two things: insert or update

1. If the object is new : it will insert it into the database.
2. If the object already exists : it will update the existing record.

**How does it know if it’s new or existing?**

> It looks at the ID field (@Id):

1. ID is null or not in DB → new → insert
2. ID exists in DB → update

<h2>1.3 Why Hibernate runs SELECT before INSERT?</h2>

- Manual ID / merge() → check if entity exists or not.
- ID generation Strategy(AUTO, TABLE, SEQUENCE).
- Relationship checks (@ManyToOne, @OneToOne).

```sql
  sql: insert into user (id, name,email) values (1, 'a','a@gmail.com');

  Hibernate: select u1_0.uid,u1_0.email,u1_0.name from user u1_0 where u1_0.uid=?
  Hibernate: insert into user (email,name,uid) values (?,?,?)

```

<h2>1.4 Custom / Dervied findBy method</h2>

<h4>Rules</h4>

- Define abstract methods in the repository.
- Format: `findBy<EntityClassPropertyName>(DataType propertyArgumentName)`.
- Return type: allow zero or more records.
- Property name: first letter uppercase after `findBy`.

Entity

```java
@Entity
public class User{
  private String name;

  @Column(name="email")
  private String emailAddress;
}
```

Repository

```java
public interface UserRepository extends JpaRepository<User, Integer> {

  List<User> findByName(String n);
  List<User> findByEmailAddress(String e);
  List<User> findByNameAndEmailAddress(String e,String n);
  List<User> findByNameOrEmailAddress(String e,String n);
}
```
[↑ Back to top](#top)
<h1 id="i">2. Insert</h1>

<h3>save()</h3>

- SQL:

```sql
  INSERT INTO user VALUES (?, ?, ?);
```

- Inserts a **single record**.

**Example:**

```java
User u = new User(1, "a", "a@gmail.com");
userRepository.save(u);
```

<h3>saveAll()</h3>

- SQL:

  ```sql
  INSERT INTO user VALUES (?, ?, ?);
  INSERT INTO user VALUES (?, ?, ?);
  ```

- Inserts **multiple records** at once.

**Example:**

```java
List<User> users = new ArrayList<>();
users.add(new User(2, "b", "b@gmail.com"));
users.add(new User(3, "c", "c@gmail.com"));
userRepository.saveAll(users);
```
[↑ Back to top](#top)
<h1 id="u">3. Update</h1>

```java
    // load and save
public void updateUserById(int id, String email) {

    Optional<User> optional = userRepository.findById(id);
    if (optional.isPresent()) {
      User user = optional.get();
      user.setEmail(email);
      userRepository.save(user);
      } 
    else System.out.println("No record Found!");
}
```
[↑ Back to top](#top)
<h1 id="r">4. Read / Select</h1>

<h3>findAll()</h3>

- SQL:

```sql
  SELECT * FROM user;
```

- Returns all records from the table.

Example:

```java
List<User> users = userRepository.findAll();
users.forEach(u -> {
  System.out.println(u.getId()+" "+u.getName()+" "+u.getEmail());
});
```

<h3>findById()</h3>

- SQL:

```sql
  SELECT * FROM user WHERE id = ?;
```


- Always points to the **identifier column** of the entity.
- Returns **single record** wrapped in `Optional<T>` no record may be present.

**Example:**

```java
Optional<User> userOptional = userRepository.findById(1L);
if (userOptional.isPresent()) {
    User u = userOptional.get();
    System.out.println(u.getId() + " " + u.getName() + " " + u.getEmail());
} else {
    System.out.println("User not found!");
}
```

<h1 id="d">5. Delete</h1>
<h3>deleteById</h3>

- The method does not return anything.
- should check record existence ( with `existsById`) before calling it to avoid exceptions.

```java
public boolean deleteUserById(int id) {
    if (userRepository.existsById(id)) {
        userRepository.deleteById(id);
        return true; // deleted
    }
    return false; // not found
}
```

<br>

[↑ Back to top](#top)   <br><br>

>**Github Code** : [CRUD Operation](https://github.com/alamgir-ahosain/Learn-Spring-Boot/tree/main/e-crud-opeation) 



