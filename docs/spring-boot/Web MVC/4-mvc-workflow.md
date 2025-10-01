
<h1 id="top"> MVC workflow</h1>


<h3>1. View layer</h3>

- The View is responsible for presenting forms/pages to the user.
- Example: Registration Form (HTML/JSP/Thymeleaf)

```html
<!DOCTYPE html>
<html>
<head></head>
<body>
    <form action=account/login method=POST>
        <input type="text" placeholder="Enter Name" name="name" id="name" required> </
        <input type="text" placeholder="Enter Email" name="email" id="email" required> </
        <input type="password" placeholder="Enter Password" name="password" id="password" required>
        <input type="submit" value="Register"/>
    </form>
</body>
</html>
```

<h3>2. Controller layer</h3> 

- Receives data from form submission, calls the Service Layer, and returns a view name with data.

```java
@Controller
public class UserController {

    @Autowired
    private UserService userService;

    // Show Registration Page
    @RequestMapping(path = "/registration", method = RequestMethod.GET)
    public String showRegistrationForm() {
        return "registration_form";
    }

    // Handle Form Submission
    @RequestMapping(path = "/account/login", method = RequestMethod.POST)
    public ModelAndView handleUserRegistration(HttpServletRequest request) {
        String name = request.getParameter("name");
        String email = request.getParameter("email");
        String password = request.getParameter("password");

        // Call Service Layer
        String msg = userService.RegisterUser(name, email, password);

        // Send response back to view
        ModelAndView mav = new ModelAndView();
        mav.setViewName("login");
        mav.addObject("message", msg);
        return mav;
    }
}

```


<h3>3. Service Layer</h3>

- Contains business logic.
- Controller pass tasks to the service.
- Service interacts with the Repository layer.

```java
@Service
public class UserService {

    @Autowired
    private UserRepository userRepository;

    public String RegisterUser(String name, String email, String password) {
        System.out.println("Name: " + name + "Email :" + email + "Password: " + password);

        if (userRepository.existsByEmail(email)) {
            return "Duplicate Email";
        }

        User u = new User();
        u.setName(name);
        u.setEmail(email);
        u.setPassword(password);
        userRepository.save(u);
        return "Created Succesfully";
    }
}
```


<h3>4. Repository Layer</h3>

- Responsible for database operations.
- Uses Spring Data JPA to interact with the DB.
```java
@Repository
public interface UserRepository extends JpaRepository<User, Integer> {
    boolean existsByEmail(String email);
}
```


<br>

[↑ Back to top](#top)   <br><br>




