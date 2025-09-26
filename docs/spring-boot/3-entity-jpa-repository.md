
<h1 id="top">Entity class and JPA Repository</h1>
<h3>📑Table of Contents</h3>

1. [Entity Class](#entity)
   - @GeneratedValue
2. [JPA Repository](#jpa)
     - CrudRepository and JpaRepository
3. [UUID(Universally Unique Identifier)](#uuid)
---

<h1 id="entity">1. Entity Class</h1>

- A POJO that represents a table in the database.
- Fields are mapped to columns in the table.
- Class must have:
    - A no-arg constructor
    - An `@Id` field (primary key)
- @Entity 
  - Table name defaults to class name (can customize with `@Table`).
  - Part of JPA (javax.persistence.Entity).
  - Required for Hibernate to manage the class.
 
  > @Id : Primary key of the entity.
  > @Column : Maps field to DB column.
  > @Table : Maps entity to DB table.

<h3>@GeneratedValue</h3>

>used with @Id to specify how the primary key should be generated.
Strategies  

 * **AUTO** : JPA chooses strategy based on DB dialect.
     - MySQL : uses AUTO_INCREMENT (IDENTITY).
     - PostgreSQL : uses SEQUENCE.
     - Oracle : uses SEQUENCE
 * **IDENTITY** : 
     - Uses DB auto-increment column.
     - ID manage by DB.
 * **TABLE** : uses an extra intermediate table for  ID generation.
    ```sql
    CREATE TABLE hibernate_sequences ( sequence_name VARCHAR(255) NOT NULL, next_val BIGINT,PRIMARY KEY(sequence_name));
    INSERT INTO hibernate_sequences (sequence_name, next_val) VALUES ('User', 1);
    ```
      Here, 
      - sequence_name = 'User' identifies the sequence for the User entity.
      - next_val is the next id Hibernate will assign.

   - **Entity with @TableGenerator**
    ```java
    @Entity
    public class User {

      @Id
      @GeneratedValue(strategy = GenerationType.TABLE, generator = "user_table_gen")
      @TableGenerator(
        name = "user_table_gen",           // generator name for Hibernate
        table = "hibernate_sequences",     // table to store sequence values
        pkColumnName = "sequence_name",    // column that stores sequence names
        valueColumnName = "next_val",      // column that stores next value
        pkColumnValue = "User",            // sequence name for this entity
        initialValue = 1,                  // starting value
        allocationSize = 1                 // increment per call
      )
      private int id;
      private String name;
      private String email;
    }
    ```


* **SEQUENCE** 
  - Uses a database sequence to generate unique IDs.
  - Supported by Oracle, PostgreSQL, DB2, etc.

  **Entity with @SequenceGenerator**
    Step 1: Create the sequence in DB
    ```sql
    -- PostgreSQL / Oracle
    CREATE SEQUENCE user_seq
    START WITH 1
    INCREMENT BY 1
    ```
    here ,
    - user_seq : sequence name.
    - START WITH 1 : first ID = 1
    - INCREMENT BY 1 : each call increases by 1

    Step 2: Use @SequenceGenerator in Hibernate

    ```java
    @Entity
    public class User {

        @Id
        @GeneratedValue(strategy = GenerationType.SEQUENCE, generator = "user_seq_gen")
        @SequenceGenerator(
            name = "user_seq_gen",      // generator name in Hibernate
            sequenceName = "user_seq", // database sequence name
            allocationSize = 1        // increment per Hibernate call
        )
        private int id;
        private String name;
        private String email;
    }
    ```
    <br>

   without use of database sequence name
    ```java
    @Entity
    public class User {

        @Id
        @GeneratedValue(strategy = GenerationType.SEQUENCE, generator = "user_seq_gen")
        @SequenceGenerator(
            name = "user_seq_gen", // generator name in Hibernate
            initialValue = 10,    // starting value of the sequence
            allocationSize = 1   // increment per Hibernate call
        )
        private int id;
        private String name;
        private String email;
    }

    ```

Entity class Example : 

```java
@Table(name="user-table")
public class User{

  @id
  @GeneratedValue(strategy = GenerationType.IDENTITY)
  @Column(name="sid")
  private int id;

  @Column
  private String name;
  //Getters and Setters 
}
```

[↑ Back to top](#top)

<h1 id="jpa">2. JPA Repository</h1>

>Pre-Defind interfaces provied by Spring JPA module.

There are two types intefaces avaiable :
1. CrudRepository (basic CRUD)  : **Parent of JpaRepository**
2. JpaRepository (CRUD + extra features like **pagination, sorting**).

**Question**: How to create JPA Repository?
- Create an interfaces by extending CrudRepository or JpaRepository .


<h3>CrudRepository and JpaRepository</h3>

>provides database operations .
- Part of Spring Data JPA.
- Provides CRUD operations for entities.
- After extends, Spring auto-generates implementation (no SQL/DAO needed).
- Common Method: `save()`, `findAll()`, `findById()`, `deleteById()`



Example:

```java
JpaRepository<T, ID>
```
Here 
- T : Entity class name
- ID :
  - Data type of identifier column of entity class.
  - use wrapper class (`Integer` for int ,`Double` for double)

Example: JPA Repository
```java 
public interface StudentRepository extends JpaRepository<User, Integer> {
    //logic here 
}
public interface StudentRepository extends CrudRepository<User, Integer> {
    //logic here 
}
```
- StudentRepository manages the Student entity.
- `JpaRepository<User, Integer>` and `CrudRepository<User, Integer>` here :
  - User : entity type.
  - Integer : type of the primary key.


[↑ Back to top](#top)
<h1 id="uuid">3. UUID(Universally Unique Identifier)</h1>

- 128-bit value, usually represented as a 36-character string like: `550e8400-e29b-41d4-a716-446655440000`
- Very useful for distributed systems where auto-increment IDs can clash.



1. Using in Java

```java
private UUID id = UUID.randomUUID();
```

2. Using in Hibernate

```java
  @Entity
  public class User {
      @Id
      @GeneratedValue(strategy = GenerationType.UUID)
      private String id;
      private String name;
  }
```

3. Using in Column (MySql)

```sql
CREATE TABLE user (
    id VARCHAR(255) PRIMARY KEY,
    name VARCHAR(255)
);
```

<br>

[↑ Back to top](#top)   <br><br>
