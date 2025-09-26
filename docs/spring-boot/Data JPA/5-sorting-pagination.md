
<h1 id="top">Sorting and Pagination</h1>
<h3>📑Table of Contents</h3>

1. [Sorting](#s)
2. [Pagination](#p)

<h1 id="s">1. Sorting</h1>

    Iterable<T>
    findAll(Sort sort)
Returns all entities sorted by the given options.



<h3>Sort by Name (Ascending)</h3>

```java
public void sortUser() {
    List<User> users = userRepository.findAll(Sort.by("name"));
    users.forEach(u -> {
        System.out.println(u.getId() + " " + u.getName() + " " + u.getEmail());
    });
}
```


<h3>Sort by Name (Descending)</h3>

```java
public void sortUserDesc() {
    List<User> users = userRepository.findAll(Sort.by(Direction.DESC, "name"));
    users.forEach(u -> {
        System.out.println(u.getId() + " " + u.getName() + " " + u.getEmail());
    });
}
```

<h3 id="p">Sort by Multiple Fields (ID + Name)</h3>

```java
public void sortUserbyIdAndName() {
    List<User> users = userRepository.findAll(Sort.by(Direction.DESC, "id", "name"));
    users.forEach(u -> {
        System.out.println(u.getId() + " " + u.getName() + " " + u.getEmail());
    });
}
```

[↑ Back to top](#top)

<h1>2. Pagination</h1>

>Pagination allows to retrieve chunks of data (pages), rather than all rows at once.
>Page start index at 0


<h3>Pageable.ofSize(n) : </h3>

>Pageable.ofSize(n) : creates a Pageable that fetches the first page (page index = 0) with n records. 
>Fetch always first page with n records.

```java
 Page<User> pageObj = userRepository.findAll(Pageable.ofSize(3));
```

<h3>PageRequest.of(...)</h3>

>create a Pageable object, which internally tells Hibernate how to apply pagination (limit and offset) to the SQL queries.

syntax:
```java
PageRequest.of(int page, int size)
PageRequest.of(int page, int size, Sort sort)
```
here ,
- page : zero-based page index (0 = first page).
- size : number of records per page.
- sort : optional for sorting.

<h2>Pagination with Sorting</h2>

```java
public void getUsersByPage(int pageNumber, int pageSize) {

  // Page<User> pageObj = userRepository.findAll(PageRequest.of(pageNumber, pageSize));
  // Page<User> pageObj = userRepository.findAll(PageRequest.of(pageNumber, pageSize, Sort.by("id")));
  // Page<User> pageObj = userRepository.findAll(PageRequest.of(pageNumber, pageSize, Sort.by(Direction.DESC, "name")));
        
    Page<User> pageObj= userRepository.findAll(PageRequest.of(pageNumber, pageSize, Direction.DESC, "id"));
    if (!pageObj.isEmpty()) {
        List<User> users = pageObj.getContent();
        sers.forEach(u -> {
        System.out.println(u.getId() + " " + u.getName() + " " + u.getEmail());

      });
   }
}
```

<br>

[↑ Back to top](#top)   <br><br>

>**Github Code** : [CRUD Operation](https://github.com/alamgir-ahosain/Learn-Spring-Boot/tree/main/e-crud-opeation) 
