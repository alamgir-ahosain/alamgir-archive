
<h1>ModelAndView</h1>

>Combines model and view in one object.

ModelAndView is a Spring MVC class used to return both:
- Model (data) : data that send to the view (like variables/messages).
- View (page) : the name of the view (JSP, Thymeleaf, etc.) to render.


```java
 @RequestMapping(path = "/account/login", method = RequestMethod.POST)
    public ModelAndView handleUserRegistration() {

        ModelAndView mav = new ModelAndView();
        mav.setViewName("login");  //login.jsp
        mav.addObject("message", msg); // 
        return mav;
    }
```
Login.jsp
```jsp
    <h1>Login Page </h1>
    <h2>${message}</h2>
```








