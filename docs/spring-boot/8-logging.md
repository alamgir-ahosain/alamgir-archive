<h1 id="top"> Logging in Spring Boot</h1>

- Process of records events, errors, messages, and execution flow of an application.
- Spring Boot provides built-in logging support using **SLF4J and Logback by default.**
- All logs can be configured via application.properties/application.yml.



<h2> Key Components</h2>

| Component      | Description                                                               |
| -------------- | ------------------------------------------------------------------------- |
| **SLF4J**      | Simple Logging Facade for Java — abstraction layer for logging frameworks |
| **Logback**    | Default logging implementation    |
| **Logger**     | for logging messages|
| **Log Levels** | Severity of logs (TRACE < DEBUG < INFO < WARN < ERROR)    |




<h2>Enabling Log Levels</h2>

By default, only **INFO and WARN** logs are active .To enable others: In application.properties

```properties
logging.level.root=INFO
logging.level.com.example=DEBUG
```

<h2>Logging Level Examples </h2>

```properties
#applies to all packages (java.*, org.*, com.*, etc.)
logging.level.root=DEBUG 

#applies to only org.framework package
logging.level.org.framework=DEBUG  

#applies to only com.alamgir package
logging.level.com.alamgir=DEBUG
```



<h2>Log display hirarchey</h2>

| Level      | Logs Displayed                                                               |
| -------------- | ------------------------------------------------------------------------- |
| TRACE    | TRACE, DEBUG, INFO, WARN, ERROR|
| DEBUG    | DEBUG, INFO, WARN, ERROR   |
| INFO    |  INFO, WARN, ERROR   |
| WARN    | WARN, ERROR    |
| ERROR    |   ERROR only    |
| OFF    | None |


[↑ Back to top](#top) 

<h2>Key Point </h2>

- TRACE : very detailed, fine-grained info
- DEBUG : debugging info, app flow
- INFO : general runtime info, milestones
- WARN : potential issues, recoverable problems
- ERROR : serious problems, app failures

Example :

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Component;

@Component
public class MyService {
    private static final Logger logger = LoggerFactory.getLogger(MyService.class);

    public void doSomething() {
        logger.trace("Trace log message");
        logger.debug("Debug log message");
        logger.info("Info log message");
        logger.warn("Warn log message");
        logger.error("Error log message");
    }
}
```




<h2>Log Output</h2>

Logs are printed in console by default.Can also write to files:
```properties
logging.file.name=app.log
logging.file.path=/logs
```

<h2>Logging Best Practices (Logback)</h2>

- Don’t log sensitive data (passwords, tokens).
- Avoid excessive logs : too many logs increase I/O load, disk usage, and reduce performance.
- Use appropriate log levels ( DEBUG for development, INFO for production)


<br>

[↑ Back to top](#top) 