<h1 id="top">Entity Relationship Mapping</h1>

<h3>📑Table of Contents</h3>


1. [One-to-One](#oto)
2. [One-to-Many](#otm)
3. [Many-to-Many](#mtm)
---

<h4>Cascade Types</h4>

- `CascadeType.PERSIST` : **Saving** the parent also saves the child.

- `CascadeType.MERGE` : **Updating** the parent also updates the child.

- `CascadeType.REMOVE` : **Deleting** the parent also deletes the child.

- `CascadeType.REFRESH` : **Refreshing** the parent also refreshes the child from the DB.

- `CascadeType.DETACH` : **Detaching** the parent also detaches the child from the persistence context.

- `CascadeType.ALL` : Applies all **of the above**.



<h3 id="oto">1. One-to-One (1:1)</h3>

1. One entity is associated with exactly one other entity.
2. Example: A User has one Profile.

**Entity example**
```java
@Entity
public class Profile{
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private int pid;
    private String email; private String phone;
    //Getters and Setters
}

@Entity
public class User{
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private int uid;
    private String name; private String gender;

    @OneToOne(cascade =CascadeType.ALL)
    private Profile profile;
    //Getters and Setters
}
```
here,
- `cascade = CascadeType.ALL` : any operation on `User` will also apply to its `Profile`




**SQL example**

```sql
CREATE TABLE profile (
    pid INT AUTO_INCREMENT PRIMARY KEY,
    phone VARCHAR(20) NOT NULL,
    email VARCHAR(255) NOT NULL,
);

CREATE TABLE user (
    uid INT AUTO_INCREMENT PRIMARY KEY,
    pid INT UNIQUE,
    name VARCHAR(255) NOT NULL,
    gender VARCHAR(255) NOT NULL,
    FOREIGN KEY (pid) REFERENCES profile(pid) ON DELETE CASCADE
);

```
here ,
- A profile must exist before inserting a user(because of FK).
- ON DELETE CASCADE( to maintain data integrity)
   - Deleting a profile deletes the user automatically 
   - but deleting a user does not delete the profile.
   - cascade only works one way here.

[↑ Back to top](#top)

<h3 id="otm">2. One-to-Many / Many-to-One (1:N / N:1)</h3>

- One entity is related to multiple other entities.
- Example: A user can place many orders, but each order belongs to one user.

```java
@Entity
public class Orders{
    @Id
    private int oid;
    private Strig name;
}

@Entity
public class User{
    @Id
    private int uid;
    private String name;

    @OneToMany(cascade = CascadeType.ALL, fetch = FetchType.EAGER)
    @JoinColumn(name = "user_uid")
    List<Orders> orders = new ArrayList<>();
}
```
here ,
**@JoinColumn(name = "user_uid")**:
- Defines the foreign key column inside the Orders table.
- the Orders table will have a column `user_uid` pointing to `User(uid)`.

SQL example: 
```sql
CREATE TABLE orders (
    oid INT PRIMARY KEY,
    name VARCHAR(255),
    user_uid INT,
    FOREIGN KEY (user_uid) REFERENCES user(uid)
);

CREATE TABLE user (
    uid INT PRIMARY KEY,
    name VARCHAR(255),
);
```

[↑ Back to top](#top)

<h3 id="mtm">3. Many-to-Many (M:N)</h3>

- Multiple entities can be associated with multiple others.
- Example : User has many Roles, and one Role has many Users
>The join table is required in a many-to-many relationship because relational databases cannot directly represent M:N relationships between two tables.

```java
@Entity
public class Roles{
    
    @Id
    private int rid;
    private String name;
}

@Entity
public class User{

    @Id
    private int uid;
    private String name;

    @ManyToMany(cascade = CascadeType.MERGE, fetch = FetchType.EAGER)
    @JoinTable(name = "user_roles_join", joinColumns = @JoinColumn(name = "uid"),
                       inverseJoinColumns = @JoinColumn(name = "rid"))
    List<Roles> roles = new ArrayList<>();
}
```

here ,
**@JoinTable:**
- Defines the third table that connects User and Roles.
- Table name: `user_roles_join`
- `joinColumns (uid)` : Refers to the User entity’s primary key.
- `inverseJoinColumns (rid)` : Refers to the Roles entity’s primary key.

SQL format:
```sql
CREATE TABLE user (
    uid INT PRIMARY KEY,
    name VARCHAR(255) NOT NULL
);

CREATE TABLE roles (
    rid INT PRIMARY KEY,
    name VARCHAR(255) NOT NULL
);

CREATE TABLE user_roles_join (
    uid INT NOT NULL,
    rid INT NOT NULL,
    PRIMARY KEY (uid, rid),
    FOREIGN KEY (uid) REFERENCES user(uid),
    FOREIGN KEY (rid) REFERENCES roles(rid)
);
```

<br>

[↑ Back to top](#top)   <br><br>
>**Github Code** : [Entity Relationship Mapping](https://github.com/alamgir-ahosain/Learn-Spring-Boot/tree/main/f-entity-relationship-mapping) 
