# Spring Data REST

Spring Data REST is built on top of the Spring Data project and makes it easy to build hypermedia-driven REST web services that connect to Spring Data repositories – all using HAL as the driving hypermedia type.

It takes away a lot of the manual work usually associated with such tasks and makes implementing basic CRUD functionality for web applications quite simple.

## 1. Configuration
### 1.1 Maven Dependencies
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-rest</artifactId>
    </dependency>
I have choosed H2 in memory database to avoid any extra database setup.
    <dependency>
        <groupId>com.h2database</groupId>
        <artifactId>h2</artifactId>
        <scope>runtime</scope>
    </dependency>

### 1.2 application.properties
Spring configures the application automatically but you can specify the configuration if you need to override them.
Here we are enabling the H2 database and configuring it's datasource url.
    spring.h2.console.enabled=true
    spring.datasource.platform=h2
    spring.datasource.url=jdbc:h2:mem:rest

## 2. Writing the Application
### 2.1 We will start by writing a domain object to represent a users:
@Entity
public class User {
    @Id
    @GeneratedValue
    private int aid;
    private String aname, tech;
}
We will include the getter and setter for all the variables.

The @Entity annotation specifies that the class is an entity and is mapped to a database table.
The @Id annotation specifies the primary key of an entity and the @GeneratedValue provides for the specification of generation strategies for the values of primary keys.

### 2.2 We will then write a repository interface:
@RepositoryRestResource(collectionResourceRel = "users", path = "users")
public interface UserRepo extends JpaRepository<User, Integer>{
}
This is an interface that allows you to perform various CRUD operations with User objects. It extends the JpaRepositroy for it's CRUD functons which takes the name of class and datatype of Id.
The @RepositoryRestResource annotation is used to customize the REST endpoint. If we decided to omit it, Spring would automatically create an endpoint at “/users instead of “/devs".

### 2.3 We can also write a custom sql queries to be executed at the start of application
To use these just create a .sql file in resources and write queries in it. It is useful for testing as H2 database is in memory database and loses data everytime it restarts.
insert into user values (1,'Jack','Java');
insert into user values (2,'Hilly','Android');
insert into user values (3,'Norma','Spring');
insert into user values (4,'Susen','python');

### 2.4 Finally, we will write a standard Spring Boot main class to initialize the application:
@SpringBootApplication
public class RestApplication {
	public static void main(String[] args) {
		SpringApplication.run(RestApplication.class, args);
	}
}
That's it! We now have a fully-functional REST API.

## 3. Accessing the REST API
We run the application and use http://localhost:8080/users enpoint using POSTMAN.

To test the CRUD operation, we can use the folowing endpoints:
GET:    http://localhost:8080/users (for all users)
        http://localhost:8080/users/1 (for user with aid=1)

POST:   http://localhost:8080/users (use the JSON body to send the data)

PUT:    http://localhost:8080/users/1 (to update the data for user with aid=1 if present, else insert)

DELETE: http://localhost:8080/users/1 (to delete the data for user with aid=1)


