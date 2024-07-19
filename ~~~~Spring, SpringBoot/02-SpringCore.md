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

## Primary

> Alternative to `@Qualifier`
> Only one per multiple implementations
> `@Qualifier` is better

```java
  @Component
  @Primary
  public class TrackCoach implements Coach{}
```

## Lazy Initialization

> By default all beans are initialized when the app starts

> tedious for all classes

> could be set globally

> spring.main.lazy-initialization=true

By defining a class to be lazy, a bean will only be initialized in these cases

- Needed for DI
- Explicitly requested

```java
@Component
@Lazy
public class TennisCoach implements Coach{}
```

## Bean Scopes

> Scope refers to lifecycle of a bean, how long does it live? how many instances? how is the bean shared?

> default scope is singleton --> spring container creates only one instance of the bean by default
> Cached in Memory
> All dependency injections for the bean will reference the same bean

```java
@Component
@Scope(ConfigurableBeanFactory.SCOPE_SINGLETON)
public class CricketCoach implements Coach{}
```

| Scope       | Description                                                            |
| ----------- | ---------------------------------------------------------------------- |
| Singleton   | Creates a single shared instance of bean.                              |
| Prototype   | Creates a new bean instance for each injection-point/container-request |
| Request     | Scoped to an HTTP web request. Only used for Web Apps                  |
| Session     | Scoped to an HTTP web session. Only used for Web Apps                  |
| Application | Scoped to a web app ServletContext. Only used for Web Apps             |
| Websocket   | Scoped to a web socket. Only used for Web Apps                         |

> check by reference to know if the beans are the same

## Bean Lifecycle methods

> Container started,beans instantiated , DI'ed, Internal Spring processing, Custom init method, Beans are ready for use
> You can add custom code during bean initialization or destruction

```java
@Component
public class CricketCoach implements Coach{

  @PostConstruct
  public void doStartUpStuff(){
    // runs after constructor
  }

  @PreDestroy
  public void cleanup()(){

  }
}
```

> For "prototype" scoped beans, Spring does not call the destroy method. Gasp!

> Thus, although initialization lifecycle callback methods are called on all objects regardless of scope, in the case of prototypes, configured destruction lifecycle callbacks are not called. The client code must clean up prototype-scoped objects and release expensive resources that the prototype bean(s) are holding.

## Java Config Beans

> No annotations, just java
> instead of @Component
> making existing 3rd party code available to spring and use it as a bean

```java
public class SwimCoach implements Coach{ }
@Configuration
public class SportsConfig{

  @Bean
  // @Bean("aquatic") --> setting the bean-id
  public Coach swimCoach(){ //bean id defaults to the method name -- by careful what you call this method
    return new SwimCoach();
  }
}
// regular injection afterwards
```
