
<h1 id="top">DDL (Data Definition Language)</h1>

>Used to define and manage the structure of database objects (tables, columns, indexes, constraints, etc).

- It changes the schema of the database, not the data itself.
- Main commands: `CREATE`, `ALTER`, `DROP`, `TRUNCATE`.

<h1>Hibernate DDL Auto Strategies</h1>

<h2>1. spring.jpa.hibernate.ddl-auto=none</h2>

- Hibernate does nothing with the schema.
- `No validation, no creation, no update`.
- It just Assumes tables are already present in DB.
- Useful in production when schema is managed manually. 


<h2>2. spring.jpa.hibernate.ddl-auto=validate</h2>

- Hibernate will check (validate) the schema against  entity mappings.
- If tables/columns don’t exist or don’t match : it throws an exception.
- `Does not create or modify tables`.
- Good when want to make sure entity class and DB schema are in sync, but don’t want Hibernate to change the DB.

<h2>3. spring.jpa.hibernate.ddl-auto=update </h2>

- Hibernate checks if the table exists for entity class
    - If yes : it uses it.
    - If no : it creates the table.(but only adds missing tables/columns, `doesn’t remove old ones`).
- Never drops or removes columns/tables.
- good when want Hibernate to auto-create missing tables/columns, but keep existing data safe.

<h2>4. spring.jpa.hibernate.ddl-auto=create </h2>

- At startup : Hibernate drops existing tables.
- Then creates new tables for all entities.
- Every restart : `tables are dropped and re-created` (so old data is lost).
- Useful for testing/prototyping.

<h2>5. spring.jpa.hibernate.ddl-auto=create-drop</h2>


- Same as create: drops + recreates at startup.
- Additionally,` drops tables again when the application stops`.
- Schema only lives for the runtime of the app.
- Common for integration/unit tests.
<br><br>
>**none** = do nothing
>**validate** = Only checks tables/columns match entities and error if mismatch.
>**update** = Creates missing tables/columns, keeps existing data, never drops.
>**create** = Drops all tables, creates fresh ones at startup (data lost).
>**create-drop** = Same as create, but also drops tables again on app shutdown.


<h1>Question: Should tables be created by JPA or by developers?</h1>

- For learning / prototyping : let JPA auto-generate (fast, easy).

- For production : tables should be created/managed by developers (manual SQL or migration tools) beacuse it's safer, more control, better performance.

> Best Practice : Always first create the DB tables (via SQL or migration tool), then map them with JPA entity classes.

<br>

[↑ Back to top](#top)   <br><br>
