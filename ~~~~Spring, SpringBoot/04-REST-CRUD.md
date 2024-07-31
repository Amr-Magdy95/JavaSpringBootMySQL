# REST CRUD APIs

## What are REST Services?

> REST is for communication between apps -- mostly uses JSON

## Spring REST Controller

```java
@RestController                       // adds rest support
@RequestMapping("/test")
public class DemoRestController{
    @GetMapping("/hello")
    public String sayHello(){
        return "Hello!";
    }
}
```

## JSON Jackson Data Binding

> Data Binding is the process of converting JSON data to Java POJO(Plain Old Java Object) or vice versa

> Data Binding Also known as Mapping, Serialization/Deserialization, Marshalling/Unmarshalling

> Jackson handles the process of data binding between JSON and Java POJO

> Make sure you have setter and getter methods defined for jackson to do its work automatically

> JSON data sent to REST controllers is converted to Java POJO

> Java POJO being returned from REST controllers is converted to JSON

## Spring REST Path Variables

```java
// GET /api/students/{studentId} // {studentId} is a path variable
@RestController
@RequestMappng("/api")
public class StudentRestController{

    @GetMapping("/students/{studentId}")
    public Student getStudent(@PathVariable int studentId){}
}

```

## Spring Boot REST Exception Handling

> in case of bad query, there's gotta be a fallback

```java
// create custom error response class -- this will be sent back to the client as JSON
public class StudentErrorResponse{
    private int status;
    private String message;
    private long timeStamp;
    // constructors, getters, setters
}
// create custom exception class
public StudentNotFoundException extends RuntimeException{
    public StudentNotFoundException(String msg){
        super(msg);
    }
}
// throw exception on bad request
if (studentId >= theStudents.size())
    throw new StudentNotFoundException("Student Id not found" + studentId);
// add exception handler method using @ExceptionHandler -- returns a response entity(wrapper for http response obj)
@RestController
@RequestMapping("/api")
public class StudentRestController{
    @ExceptionHandler
    public ResponseEntity<StudentErrorResponse> handleException(StudentNotFoundException exe){
        StudentErrorResponse  err = new StudentErrorResponse();

        err.setStatus(HttpStatus.NOT_FOUND.value());
        err.setMessage(exe.getMessage());
        err.setTimeStamp(System.currentTimeMillis());

        return new ResponseEntity<>(err, HttpStatus.NOT_FOUND);
     }
}
```

> How about a catch all exception handler?

```java
// function overloading
@ExceptionHandler
public ResponseEntity<StudentErrorResponse> handleException(Exception exc){
    StudentErrorResponse  err = new StudentErrorResponse();

    err.setStatus(HttpStatus.BAD_REQUEST.value());
    err.setMessage(exe.getMessage());
    err.setTimeStamp(System.currentTimeMillis());

    return new ResponseEntity<>(err, HttpStatus.BAD_REQUEST);
}
```

## Spring Boot REST  Global Exception Handling
> @ControllerAdvice is similar to an interceptor -- AOP(Aspect-Oriented Programming)
> used to pre-process requests to controllers and post-process responses to handle exceptions
> best practice
```java
// remove exception handling from the rest services
//StudentRestExceptionHandler.java
@ControllerAdvice
public class StudentRestExceptionHandler{

    @ExceptionHandler
    public ResponseEntity<StudentErrorResponse> handleException(Exception exc){
        StudentErrorResponse  err = new StudentErrorResponse();

        err.setStatus(HttpStatus.BAD_REQUEST.value());
        err.setMessage(exe.getMessage());
        err.setTimeStamp(System.currentTimeMillis());

        return new ResponseEntity<>(err, HttpStatus.BAD_REQUEST);
    }
}
```
## Spring Boot REST API Design - Best Practices
> convention is to use plural form of entity --> /api/employees/
> Full CRUD

### Bad Practices`
- Don't use the actions in the endpoint, instead use http methods to assign crud actions
    - /api/addEmployee BAAAAAAAD vs POST /api/employees    GOOOOOD
## Spring Boot Application Architecture

Rest Controller <---> Service <---> DAO + Repo <---> Entity <---> Actual DB
### Entity
> Define fields, constructors(no-arg-constructor REQUIRED BY JPA), getters/setters, toString
### DAO + Repo
> Create DAO interface and provide implementation be it by entityManager or otherwise
### Service Layer
> Between employee controller and employee DAO

> Uses Service Facade design pattern

> Intermediate Layer for custom business logic

> Used to integrate data from multiple sources (DAOs/Repositories) -- in single crud it only delegates, needed for larger projs

> Best Practice

> @Service, @Repository, @RestController sub-annotations of @Component

```java
public interface EmployeeService{
    List<Employee> findAll();
}

@Service
public class EmployeeServiceImpl implements EmployeeService{
    // inject EmployeeDAO

    @Override
    public List<Employee> findAll(){
        return this.employeeDAO.findAll();
    }
}
// this will later be used in rest controller 
```


