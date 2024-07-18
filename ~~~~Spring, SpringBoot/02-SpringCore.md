# Spring Core

## Inversion of Control (IoC)

> Outsourcing the construction and management of objects to object/bean factory using a configuration

> Spring container works as an object factory

> A Spring Bean is just a regular java class that is managed by spring

> Java Annotations and Java Source Code for configuration -- xml file configuration is deprecated

Spring container has two mains functions

- create and manage objects (Inversion of Control)
- Inject object dependencies (Dependency Injection)

## Dependency Injection (DI)

> Makes use of the dependency inversion principle

> The client delegates to another object(Spring Container) the responsibility of managing and providing its dependencies

Two Recommended types of injection

- Constructor Injection when you have required dependencies -- recommended by spring dev team
- Setter Injection when you have optional dependencies -- if dependency not provided, app provides default logic

For dependency injection, spring can use autowiring `@Autowired` annotation

- looks for class or interface that matches type and injects automatically
- Spring scans for ` @Component` to autowire -- injecting a class that implements a given interface
- `@Component` marks the class as a spring bean, makes it a candidate for DI

```java
  public interface Coach{
    String getDailyWorkout();
  }

  @Component
  public class CricketCoach implements Coach{
    @Override
    public String getDailyWorkout(){
      return "Practice for 15 minutes";
    }
  }

  @RestController
  public class DemoController{
    private Coach myCoach;

    @Autowired
    public DemoController(Coach theCoach){
      this.myCoach = theCoach;
    }
  }
```

## Component Scanning

> Spring will scan your java classes for annotations, and automatically register beans in spring container

@SpringBootApplication = @EnableAutoConfiguration + @Configuration + @ComponentScan

- @SpringBootApplication(scanBasePackages = {"com.amr.demo", "com.amr.utils"})

## Setter Injection

> Could be used on any method tbh

```java
@RestController
public class DemoController{
  private Coach coach;

  @Autowired
  public void setCoach(Coach theCoach){
    this.coach = theCoach;
  }
}
```

## Field Injection

> Not Recommended by spring dev team

makes code harder to unit test

## Qualifiers

> in case of multiple implementation of the autowired interface we can use `@Qualifier`

Multiple implementation, which one to inject?

- specify bean id , same as class, first letter lowercase
- @Qualifier("cricketCoach")

```java
@RestController
public class DemoController{
  private Coach theCoach;
  @Autowired
  public DemoController(@Qualifier("cricketCoach") Coach theCoach){
    // 3 implementations for Coach interface
    this.theCouch = theCouch;
  }
}
```

- Primary
  - Alternate to Qualifier
  - @Component @Primary for a certain implementation
  - @Qualifier is better
- Lazy Initialization
  - By default all beans are initialized
  - @Component @Lazy
  - tedious for all classes
  - could be set globally
  - spring.main.lazy-initialization=true
- Bean Scopes
  - Scope refers to lifecycle of a bean, how long does it live? how many instances? how is the bean shared?
  - default scope is singleton
  - @Component @Scope(configurableBeanFactory.SCOPE_SINGLETON)
  - singleton, prototype, request, session, application, websocket
- Bean Lifecycle methods
  - Container started, DI'ed, Spring processing, Custom init method,
  - You can add custom code during bean initialization or destruction
  - @PostConstruct @PreDestroy
- Java Config Beans
  - No annotations, just java
  - @Configuration class @Bean to config bean, inject bean into controller
  - instead of @Component
  - making existing 3rd party code available to spring and use it as a bean
  -

```

```
