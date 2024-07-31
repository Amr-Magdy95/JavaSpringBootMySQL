# Hibernate/JPA CRUD

## Big Picture

Just a reminder there're three layers here

- Spring JDBC
- Spring Data JDBC
- JPA with vendor implementation

## Hibernate/JPA Overview

> Hibernate is a framework for saving java objects into a db.

> A backend talks to a db, hibernate makes this process of communication easier



Hibernate minimizes the amount of JDBC code you have to develop as it handles low-level sql code

Provides ORM --> mapping java classes to database tables, using xml or java annotations


> JPA is Jakarta Persistence API

Standard API for ORM
Only a specification --> defines a set of interfaces and requires an implementation to be usable.
  - Hibernate is one-way to provide said implementations
  - Other jpa vendors: MyBatis, EclipseLink, etc
It's a specification that defines a standard way to manage relational data in Java applications.
entity manager is a special jpa helper object
At its core, JPA is an ORM tool. It acts as a bridge between the object-oriented world of your Java code and the relational world of databases. This lets you work with data using Java objects that represent your real-world entities, instead of writing complex SQL statements.
JPA relies on an ORM provider (like Hibernate or EclipseLink) to implement its specifications.
These providers take care of:
- Mapping Java classes to database tables
- Translating object operations to database queries
  Hibernate provides implementation for said interfaces, so does eclipselink
  https://www.jcp.org/en/jsr/detail?id=338
  https://hibernate.org/orm/documentation/6.5/

```java
// jpa has a language for queries
TypedQuery<Student> theQuery = entityManager.createQuery("from Student", Student.class); //jpql
```
## Hibernate, JPA and JDBC
The relationship between Hibernate/JPA and JDBC
- Hibernate/JPA uses JDBC for all DB communication; Hibernate/JPA is just another layer of abstraction on top of JDBC

## Setting up JSB project
Automatic Data Source Configuration
- In Spring Boot, Hibernate is the default implementation of JPA
- EntityManager is the main component for creating queries, EntityManager is from JPA
- Based on Configs, JSB will automatically create the beans
  - DataSource class
  - EntityManger class
  - Then we can inject those into our app, e.g DAO
- Spring will automatically configure DataSource based on the entries from pom file
  - Also DB connected info from application.properties
```java
spring.datasource.url=jdbc:mysql://localhost:3306/student_tracker
spring.datasource.username=springstudent
spring.datasource.password=springstudent
spring.main.banner.mode=off
logging.level.root=warn

```  


## JPA Annotations Overview

### Terminology
| Annotation  | Description |
| ------------- | ------------- |
| @Entity  | Annotates a java entity class that is mapped to a db table  |
| @Table  | Annotates a java entity class that is mapped to a db table  |
| @Id  |  |
| @Column(name="db_col_name")  | optional but recommended |

### Entity Class
Requirements
  - Must be annotated with @Entity
  - Must have a public/protected no-args constructor at a minimum
    - can have other constructors
> Reminder, class with no constructors creates no-args constructor for free
> class with constructors with args DOES NOT create no-args constructor for free -- must declare it explicitly

```java
@Entity
@Table(name="student") // takes the name of the db table
public class Student{
  @Id                                                    // marks as Primary key
  @GeneratedValue(strategy=GenerationType.IDENTITY)      // mark this is managed by the db
  @Column(name="id")
  private int id;

  @Column(name="first_name") //first_name is the name of the column in the db
  private String firstName;

  // getters & setters, constructors, toString
  // you can use lombok if you want
}
```
### ID Generation Strategies
| Type  | Description |
| ------------- | ------------- |
| .AUTO  | pick the approp. strategy of the db  |
| .IDENTITY  | Assign PK using db identity column  |
| .SEQUENCE  | Assign PK using db sequence  |
| .TABLE  | Assign PK using db table  |
| .UUID  | Assign PK using uuid  |

### Custom Generation Strategies
> Create an implementation of the org.hibernate.id.IdentifierGenrator interface
> Override the method public Serializable generation();

## Saving a Java Object with JPA (Create)
> Create a StudentDAO

> DAOs are responsible for interfacing with the DB -- common design pattern

> DAO needs a JPA EntityManager

> JPA needs a Data Source, DS define DB connection info, JPA EntityManager and DataSource are auto. created by spring from application.properties file, we can then inject EntityManager into the DAO

StudentDAO <------> EntityManager <------> DataSource <------> DB

### What about JpaRepository?
> Spring Data JPA has a JpaRepository interface
> This provides DB access with minimal coding

> We'll use both, knowing both will help in future projects

> EntityManager for low-level control and flexibility,JpaRepository for high-level of abstraction

```java
public interface StudentDAO{
  void save(Student theStudent);
}

@Repository          // sub-annotation of @Component --> specialized for DAOs implementations
public class StudentDAOImpl implements StudentDAO{
  private EntityManager entityManager;

  @Autowired
  public StudentDAOImpl(EntityManager theEntityManager){
    entityManager = thisEntityManager;
  }
  
  @Override
  @Transactional // @Transactional automatically begins and ends jpa transactions
  public void save(Student theStudent){
    entityManager.persist(theStudent);
  }
}

```
## Primary Keys
> MySQL handles the AUTO_INCREMENT
## Changing the Index of MySQL AUTO_INCREMENT
```sql
ALTER TABLE student_tracker.student AUTO_INCREMENT=3000;
TRUNCATE student_tracker.student;  -- resets AUTO_INCREMENT value to start at 1 
```

## Reading Objects with JPA
> Querying for a single object
```java
// if not found, it returns null
// not need to use @Transactional as this doesn't change data in the db
Student myStudent = entityManager.find(Student.class, 1);   //.find(Entity Class, Primary Key)
```

## Querying Objects with JPA
> Querying for multiple objects

> JPA Query Language (JPQL)
> Similar to SQL, JPQL is based on entity name and entity fields

```java
// Student in the string the is the entity name, THIS IS NOT THE NAME OF THE DB table
TypedQuery<Student> theQuery = entityManager.createQuery("FROM Student", Student.class); 
List<Student> students = theQuery.getResultList();
```

```java
// lastName is jpa entity field NOT THE COLUMN FROM THE DB
TypedQuery<Student> theQuery = entityManager.createQuery("FROM Student WHERE lastName=`Doe`", Student.class);
TypedQuery<Student> theQuery1 = entityManager.createQuery("FROM Student WHERE lastName=`Doe` OR firstName=`john`", Student.class);
TypedQuery<Student> theQuery2 = entityManager.createQuery("FROM Student WHERE email LIKE `%d@gmail.com`", Student.class);
// Named Parameters
TypedQuery<Student> theQuery2 = entityManager.createQuery("FROM Student WHERE lastName=:theData", Student.class);
theQuery.setParameter("theData", userInsertedLastName);
theQuery.getResultList;

```
###  JPQL - The SELECT Clause
> The query examples so far didn't provide a SELECT clause

> Because, the hibernate implementation behind the scenes is lenient and Allows HQL(Hibernate Query Language)

> For strict JPQL, the select clause is required
```java
// strict jqpl
TypedQuery<Student> theQuery = entityManager.createQuery("select s FROM Student s", Student.class);
// s is an identification variable, provides a reference to the student entity object

// examples on strict jqpl
TypedQuery<Student> theQuery = entityManager.createQuery("select s FROM Student s WHERE s.email LIKE ...", Student.class);

```

### Actual Implementation

```java
public interface StudentDAO{
  List<Student> findAll();
}

@Repository
public class StudentDAOImpl implements StudentDAO{
  private EntityManager entityManager;

  @Override
  public List<Student> findAll(){
    TypedQuery<Student> theQuery = entityManager.createQuery("SELECT s FROM Student s ORDER BY lastName", Student.class);
    List<Student> results = theQuery.getResultList();
    return results;
  }
}
```
## Updating an Object
> Finding the obj using entityManager, use a setter, then merge using entity manager
```java
Student theStudent = entityManager.find(Student.class, 1);
theStudent.setFirstName("Scooby");
entityManager.merge(theStudent);
// OR
int numOfRowsUpdated = entityManager.createQuery("UPDATE Student SET lastName=`Tester`")
  .executeUpdate();
```
## Delete an Object
```java
Student theStudent = entityManager.find(Student.class, 1);
entityManager.remove(theStudent);
// you can do batch delete with a query
int numOfRowsDeleted = entityManager.createQuery("DELETE FROM Student WHERE lastName=`Smith`")
  .executeUpdate();
// you can delete all students
```

## Create DB table from Java Code
> Hibernate provides the option to create db tables from java code
> Based on Annotations

```java
// application.properties
spring.jpa.hibernate.ddl-auto=create  // when you run the app, hibernate will drop the tables and create them
// create-drop, drop, none, create, validate, update
// this is fine for dev and testing
// Don't do it on production dbs
```
