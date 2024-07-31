# Spring Boot Overview

> Spring is a framework, provides helper classes and annotations

> Spring projects are really hard to configure
> Spring Boot minimizes manual configuration
> Spring Boot uses Spring and makes it easier to use Spring

> Maven is a project management tool

POM.xml

- Project Object Model
- Configuration file for maven
- metadata, dependencies, plugins

## Spring Boot DevTools

> automatic restart on code update

## Spring Boot Actuator

> monitoring and managing app
> checking app health
> accessing app metrics

> Spring Boot Actuator exposes endpoints to monitor and manage app

Add the dependency to the POM.xml file

- all endpoints are prefixed with /actuator
- by default, only /health is exposed
- to expose /info --> ..../application.properties -->
  - management.endpoints.web.exposure.include=health,info || or \* to expose all || could replace 'include' with 'exclude'
  - management.info.env.enabled=true
  - info.app.name=App
  - info.app.description=description
  - info.app.version=1.0.0
- https://docs.spring.io/spring-boot/reference/actuator/endpoints.html#page-title
- By adding spring security you automatically have security on rest endpoints
  | Name | Description |
  | ------------ | --------------------------------------------------------- |
  | /health | Health info about your app -- to se whether is up or down |
  | /info | Gives info about the app |
  | /auditevents | Audit events for app |
  | /beans | lists all beans that are registered in app context |
  | /mappings | lists all @RequestMapping paths |

## Spring Boot Actuator Security

- Option #1 default username is 'user' and password is generated in the console
- Option #2
  - spring.security.user.name=scott
  - spring.security.user.password=tiger
-

## Running Spring Boot from the Command Line

- ./mvnw package
- Two options
  - java -jar mycoolapp.jar
  - mvn clean compile test
  - ./mvnw.cmd spring-boot:run // for windows
  - ./mvnw.ssh spring-boot:run // for linux

## Custom app properties

.../application.properties

```java
  coach.name=Mouse
```

```java
@RestController
public class FunRestController{
  @Value("${coach.name}")
  private String coachName;

}
```

## App properties

- logging.level.\*
- server.\*
- actuator props
- spring.security.\*
- spring.jdbc.\*
- https://docs.spring.io/spring-boot/appendix/application-properties/index.html
