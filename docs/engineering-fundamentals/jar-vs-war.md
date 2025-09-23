
<h1>JAR vs WAR </h1>

1. Full Form:
    - JAR = Java ARchive
    - WAR = Web ARchive

2. Purpose
    - JAR = Standalone Java apps or libraries
    - WAR = Web applications for servers

3. Contents
    - JAR = .class files, resources, META-INF
    - WAR = Web files (HTML/JSP), WEB-INF, classes, libraries

4. Deployment:
    - JAR = Run with java -jar
    - WAR = Deploy on web server (Tomcat, JBoss)

5. Execution:
    - JAR = Executable (if Main-Class present)
    - WAR = Server-managed, not directly executable

6. Scope:
    - JAR = Desktop/CLI apps, libraries
    - WAR = Web apps (servlets, JSP, REST APIs)

7. Relationship:
    - WAR often contains JARs inside WEB-INF/lib.
    - JAR = Java app/library
    - WAR = Web app for server