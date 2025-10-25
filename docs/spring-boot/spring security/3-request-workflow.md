

<h1 id="top">JWT Authentication Workflow in Spring Boot Security</h1>

This document explains the **step-by-step workflow** of JWT (JSON Web Token) authentication in a Spring Boot application using Spring Security.

***

<h1>1. Defining User Credentials (Entity Class)</h1>

- Create an entity class that implements `UserDetails`.  
- This allows Spring Security to treat your custom `User` entity as a core security object.

<Details>
<Summary>code</Summary>

```java
@Entity
@Table(name = "user_info")
public class User implements UserDetails {

    @Id
    @Column(unique = true, nullable = false)
    private String email;

    private String name;

    @Column(nullable = false)
    private String password;

    @Override
    public Collection<? extends GrantedAuthority> getAuthorities() {return null; // No roles for now}

    @Override
    public String getUsername() {   return email;  }

    //getters ans setters
}
```

</Details>

Key Points:
- The `email` acts as the **username** for authentication.
- Spring Security uses this `UserDetails` interface to encapsulate user information.


<h1>2. Loading User Information (UserDetailsService)</h1>

Spring Security uses an implementation of `UserDetailsService` to fetch user details from the database for authentication.

<Details>
<Summary>Code</Summary>

```java
@Service
public class UserAuthenticationServiceImpl implements UserDetailsService {

    Logger logger = LoggerFactory.getLogger(UserAuthenticationServiceImpl.class);

    @Autowired
    UserRepository repository;

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        logger.info("UserAuthenticationServiceImpl : loading data from DB of username  : "+username);
        User user = repository.findByEmail(username);
        if (user == null) { 
            ogger.info("UserAuthenticationServiceImpl : username is not found in DB");
            throw new UsernameNotFoundException("Invalid User Name : "+username);
            }

            return user;
	}
}
```
</Details>
 
Workflow:
1. When a user tries to log in, the `loadUserByUsername()` method is automatically called.
2. The method retrieves the user by email from the database.
3. If found, it returns a valid `UserDetails` object.

[↑ Back to top](#top) 

<h1>3. Security Configuration</h1>

The security configuration defines which endpoints are **public** and which are **protected**, and it integrates the JWT filter before the default authentication filter.

<Details>
<Summary>Code</Summary>

```java
@Configuration
public class AppSecurityConfig {

    Logger logger = LoggerFactory.getLogger(AppSecurityConfig.class);

    @Autowired
    JWTTokenFilter jwtTokenFilter;

    @Bean
    AuthenticationManager getAuthenticationManager(AuthenticationConfiguration authenticationConfiguration) throws Exception {
        logger.info("AppSecurityConfig : Initilizing Bean AuthenticationManager");
        return authenticationConfiguration.getAuthenticationManager();
        }

    @Bean
    BCryptPasswordEncoder getBCryptPasswordEncoder() {
     logger.info("AppSecurityConfig : Initilizing Bean BCryptPasswordEncoder");
      return new BCryptPasswordEncoder();
      }
	
    @Bean
    public SecurityFilterChain getSecurityFilterChain(HttpSecurity security) throws Exception {
        logger.info("AppSecurityConfig : Configuring SecurityFilterChain Layer  of URl patterns");
        security.csrf(csrf -> csrf.disable())
                .cors(cors -> cors.disable())
                .authorizeHttpRequests(
                    auth -> auth.requestMatchers("/public/**")
                                .permitAll()
                                .anyRequest()
                                .authenticated())
                .addFilterBefore(this.jwtTokenFilter, UsernamePasswordAuthenticationFilter.class);
		return security.build();
	}
}
```
</Details>

Workflow:
- `/public/**` endpoints are accessible without authentication.
- All other routes require a valid JWT token.
- `JWTTokenFilter` is added before `UsernamePasswordAuthenticationFilter` in the chain.


[↑ Back to top](#top) 

<h1>4. JWT Token Validation (Filter Layer)</h1>

Spring’s `OncePerRequestFilter` ensures each HTTP request passes through the JWT validation logic once per request.

<Details>
<Summary>Code</Summary>

```java
@Component
public class JWTTokenFilter extends OncePerRequestFilter {

    Logger logger = LoggerFactory.getLogger(JWTTokenFilter.class);

    @Autowired
    JwtTokenHelper jwtTokenHelper;

    @Autowired
    UserAuthenticationServiceImpl authenticationService;

    @Override
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain)   throws ServletException, IOException {

        logger.info("JWTTokenFilter : validation of JWT token by OncePerRequestFilter");
       // Step 1: Extract the Authorization header (JWT Token)
        String token = request.getHeader("Authorization");

        logger.info("JWT token : " + token);
        String userName = null;
        // Step 2: Extract username from the token (if token exists)
        if (token != null) {
            userName = this.jwtTokenHelper.extractUsername(token);
            logger.info("JWTTokenFilter : Request Token come from an User : " + userName);
        } else {
            logger.info("ToKen is Misisng. Please Come with Token");
        }

        // Step 3: Validate token and authenticate user
        if (userName != null && SecurityContextHolder.getContext().getAuthentication() == null) {

            logger.info("JWTTokenFilter : getting user info from DB");
             // Step 4: Load user details from database
            UserDetails userDetails = this.authenticationService.loadUserByUsername(userName);
            logger.info("JWTTokenFilter :  Now Validating  the token ");
                        
            // Step 5: Validate the token
            Boolean isValidToken = this.jwtTokenHelper.isValidToken(token, userDetails.getUsername());
            logger.info("JWTTokenFilter : Is it Valid Token : " + isValidToken);
                     
            // Step 6: If valid, set authentication in SecurityContext
            if (isValidToken) {

                logger.info("JWTTokenFilter : Setting Security Context for that username");
                UsernamePasswordAuthenticationToken authenticationToken = new UsernamePasswordAuthenticationToken(userDetails, null, userDetails.getAuthorities());
                authenticationToken.setDetails(new WebAuthenticationDetailsSource().buildDetails(request));
                SecurityContextHolder.getContext().setAuthentication(authenticationToken);
            }

        }
        // Step 7: Continue the filter chain
        filterChain.doFilter(request, response);
    }

}
```
</Details>

Workflow

<h4>1. Extract Token</h4> 

- Retrieve the `Authorization` header from the HTTP request.

<h4>2. Extract Username:</h4> 

- Get the username encoded inside the JWT.

<h4>3. Check if User Needs Authentication</h4>

```java
    If username !=null  && SecurityContextHolder.getContext().getAuthentication() == null
```
- Proceed to authentication **only if**:
     - `userName` is extracted successfully from the token, **AND**
    - The current request does **not already have** an authenticated user in `SecurityContextHolder`.

<h4>4. Load User Details from Database</h4>

- The filter calls `UserDetailsService.loadUserByUsername()` to fetch user details from the database.
- This confirms the user exists in the system; if not, token validation is skipped.

<h4>5.  Validate the Token</h4>

- if token is valid
    - Set Authentication in SecurityContext: Build a `UsernamePasswordAuthenticationToken` and set it in the `SecurityContextHolder`.
    - This marks the user as authenticated for the current request.
		


<h4>6. When Token Is Missing or Invalid</h4>

 - **Public API Request**: Skips token validation and proceeds.
-  **Protected API Request**:
    - If token is **missing** or **invalid**, `SecurityContextHolder` remains empty.
    - Spring Security denies access and returns **`401 Unauthorized`**.

<h4>  7: Continue the Filter Chain</h4>

- After token validation (or if no token exists), pass the request to the next filter in the chain.
- If the user is authenticated, they can access protected resources.
- If not, they'll receive a `401 Unauthorized` response (handled by Spring Security)





[↑ Back to top](#top) 

<h1>5. User Login Flow (Token Generation)</h1>

When a user logs in with valid credentials, the system authenticates and issues a JWT.

<Details>
<Summary>Code</Summary>

```java
@PostMapping("/public/login")
public ResponseEntity<UserLoginResponse> loginUser(@Valid @RequestBody UserLoginRequest loginRequest) {

   this.doAuthenticate(loginRequest.getEmail(), loginRequest.getPassword());
   String token = this.jwtTokenHelper.createToken(loginRequest.getEmail());
   eturn ResponseEntity.ok(new UserLoginResponse(loginRequest.getEmail(), token));
 }


public void doAuthenticate(String username, String password) {

   logger.info("Authentication of User credentials");
   UsernamePasswordAuthenticationToken credentials = new UsernamePasswordAuthenticationToken(username, password);

    try { authenticationManager.authenticate(credentials);  } 
    catch (BadCredentialsException e) {throw new RuntimeException("Invalid username and password");   }
 }
```
</Details>

### Flow:
1. User sends a **POST** request with `{ "email": "...", "password": "..." }`.
2. Credentials are verified using the authentication manager.
3. If valid, JWT is generated via `JwtTokenHelper.createToken()`.
4. The token is returned to the client for future authenticated requests.


<h1>6. Scenario</h1>

| Scenario                         | Behavior                                                                 |
|----------------------------------|--------------------------------------------------------------------------|
| **Token is valid**               | User authenticated → `SecurityContextHolder` populated → Access granted  |
| **Token is invalid or expired**  | Authentication fails → `SecurityContextHolder` remains empty → **401 Unauthorized** |
| **Token is missing**             | Public API: Access granted<br>Protected API: **401 Unauthorized**        |
| **Token present but user not in DB** | `UsernameNotFoundException` thrown → **401 Unauthorized**               |

<br>


<h1>7. Summary Flow Diagram</h1>

![JWT Request wokfloe](<JWT .drawio (1).png>)


<br>

[↑ Back to top](#top) 


>**Github Code** : [JWT - Token Based Authentication](https://github.com/alamgir-ahosain/Learn-Spring-Boot/tree/main/l-spring-boot-security-jwt)