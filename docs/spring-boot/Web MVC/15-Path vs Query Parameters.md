
<h3 id="top">📑Table of Contents</h3>

1. [Path variabless / path parameters](#p)
2. [Query Parameters /request parameters](#q)
3. [Combination of Path and Query Parameters](#c)
4. [Path vs Query Parameters](#pa)

---

<h1 id="p">1. Path variabless / path parameters</h1>

- Used to pass resource identifiers in the URI path.
- Always part of the URI and enclosed in curly braces {}.
- Binding values to method parameters using `@PathVariable`.
- Suitable for public data or mandatory identifiers.
- name attribute optional if parameter name matches the path variable.


Example  
```java
@RequestMapping(path = "/user/{email}", method = RequestMethod.GET)
public void readByEmail(@PathVariable(name="email") String emailId){}

@RequestMapping(path = "/user/info/{id}", method = RequestMethod.GET)
public void readById(@PathVariable int id){}
```
Access :

```java
GET  localhost:8080/user/abc@gmail.com
GET  localhost:8080/user/info/12
```

<h3>Multiple Path Variables</h3>

```java
 @RequestMapping(path = "/user/{id}/{email}", method = RequestMethod.GET)
 public void read(@PathVariable int id ,@PathVariable String email){}
```

Access :

```java
GET  localhost:8080/user/info/12/abc@gmail.com
```

[↑ Back to top](#top)

<h1 id="q">2. Query Parameters /request parameters</h1>

- Passed via the URI after `?` in `key=value` form.
- Multiple query params are separated by `&`.
- Use` @RequestParam` to bind values to method parameters.
- By default, parameters are mandatory (can be optional with `required=false`).

Example  
```java
@RequestMapping(path = "/user/info", method = RequestMethod.GET)
public void read(@RequestParam String country) {}
```

Access :

```java
GET  localhost:8080/user/info?country=Bangladesh
```


<h3>Multiple Parameters</h3>

```java
@RequestMapping(path = "/user/info", method = RequestMethod.GET)
public void read(@RequestParam String country, @RequestParam String city) {}
```

Access :
```java
GET  localhost:8080/user/info?country=Bangladesh&city=Tangail
```


<h3>Optional Parameters </h3>

```java
@RequestMapping(path = "/user/info", method = RequestMethod.GET)
public void read(@RequestParam(name="country" ,required=false) String countryName, 
                 @RequestParam(required=false) String city){

     if(countryName!=null && city!=null){getByCountryAndCity()}
     else if(countryName!=null && city==null){getByCountry()}
     else if(countryName==null && city!=null){getByCity()}
     else if(countryName==null && city==null){getAllUser()}
}
```

Access :

```java
/user/info                     → Get all users
/user/info?country=Bangladesh  → Get users filtered by country
/user/info?city=Tangail        → Get users filtered by city
/user/info?country=Bangladesh&city=Tangail → Get users filtered by both country and city
```
 
<h1 id="c"> 3. Combination of Path and Query Parameters</h1>

 
 ```java
 @RequestMapping(path = "/user/{id}", method = RequestMethod.GET)
public void getUser(
    @PathVariable int id,
    @RequestParam(required=false) String country,
    @RequestParam(required=false) String city
) {}
```

Access :
```java
/user/12                     → Get user with id 12
/user/12?country=Bangladesh  → Get user 12 filtered by country
/user/12?city=Tangail        → Get user 12 filtered by city
/user/12?country=Bangladesh&city=Tangail → Both filters applied
```

<h1 id="pa">4. Path vs Query Parameters</h1>

- if having limited no of properties and all are mandatory then go with path parameters.
- when mandatory and optional properties then go with query parametes. 



<br>

[↑ Back to top](#top)   <br><br>
