# Spring Certified Professional 2023 - Study Guide

## Module 3 - Data Management: JDBC, Transactions, Spring Data JPA

***

## 1. What is the difference between checked and unchecked exceptions?

#### Checked Exceptions

In general, checked exceptions represent errors outside the control of the program. For example, the constructor of FileInputStream throws FileNotFoundException if the input file does not exist.

**Java verifies checked exceptions at compile-time.**

Therefore, we should use the throws keyword to declare a checked exception:

```java
private static void checkedExceptionWithThrows() throws FileNotFoundException {
    File file = new File("not_existing_file.txt");
    FileInputStream stream = new FileInputStream(file);
}
```
We can also use a try-catch block to handle a checked exception:


```java
private static void checkedExceptionWithThrows() throws FileNotFoundException {
    File file = new File("not_existing_file.txt");
    FileInputStream stream = new FileInputStream(file);
}
```
Some common checked exceptions in Java are IOException, SQLException and ParseException.

The Exception class is the superclass of checked exceptions, so we can create a custom checked exception by extending Exception:

```java
public class IncorrectFileNameException extends Exception {
    public IncorrectFileNameException(String errorMessage) {
        super(errorMessage);
    }
}
```

#### Unchecked Exceptions

If a program throws an unchecked exception, it reflects some error inside the program logic.

For example, if we divide a number by 0, Java will throw ArithmeticException:

**Java does not verify unchecked exceptions at compile-time.** Furthermore, we don't have to declare unchecked exceptions in a method with the throws keyword. And although the above code does not have any errors during compile-time, it will throw ArithmeticException at runtime.

Some common unchecked exceptions in Java are NullPointerException, ArrayIndexOutOfBoundsException and IllegalArgumentException.

The RuntimeException class is the superclass of all unchecked exceptions, so we can create a custom unchecked exception by extending RuntimeException:

```java
public class NullOrEmptyException extends RuntimeException {
    public NullOrEmptyException(String errorMessage) {
        super(errorMessage);
    }
}
```
_Reference: https://www.baeldung.com/java-checked-unchecked-exceptions_

#### Why does Spring prefer unchecked exception?

- Reduces cluttered code
- Reduces coupling between callee and caller
- Give you more freedom - do you want to handle the exception or not, how to handle it etc.

<p align="center">The hierarchy of Exceptions in the Java</p>


![](images/exception-hierarchy.png)

#### What is the data access exception hierarchy?

Object -> Throwable -> Exception -> RuntimeException -> NestedRuntimeException  -> DataAccessException

This exception hierarchy aims to let user code find and handle the kind of error encountered without knowing the details of the particular data access API in use (e.g. JDBC). Thus, it is possible to react to an optimistic locking failure without knowing that JDBC is being used.

As this class is a runtime exception, there is no need for user code to catch it or subclasses if any error is to be considered fatal (the usual case).

_Source: https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/dao/DataAccessException.html_

***

## 2. How do you configure a DataSource in Spring? Which bean is very useful for development/test databases?

Configuration of Data Source in Spring is dependent on the type of application that is executed.

#### Standalone

Data Source is configured in @Configuration class and is created as a bean of one of the supported data source types.

~~~java
@Configuration
public class DataSourceConfig

    @Bean
    public DataSource dataSource
        BasicDataSource basicDataSource = new BasicDataSource();
        basicDataSource.setDriverClassName("org.hsqldb.jdbcDriver");
        basicDataSource.setUrl("jdbc:hsqldb:mem:locahost");
        return basicDataSource;
~~~

#### Spring Boot

Data Source is configured through _application.properties_

```
spring.datasource.url=jdbc:hsqldb:mem:locahost
spring.datasource.driver-class-name=org.hsqldb.jdbcDriver
```

#### Application Server

Data Source should be fetched from JNDI via JndiDataSourceLookup / JndiTemplate. Application Server is responsible for creating and managing the data source requested in resources configurations of deployment descriptors. 

#### Which bean is very useful for development/test databases?

When working with development/test databases, the following beans are very useful:

- EmbeddedDatabaseBuilder - Allows you to easily configure H2/HSQLDB embedded database with schema/data initialization scripts
- DataSourceInitializer / ResourceDatabasePopulator - Allows you to use schema/data initialization scripts without usage of _EmbeddedDatabaseBuilder_

<br>

***

## 3. What is the Template design pattern and what is the JDBC template?

The Template Design Pattern is a behavioural design pattern that can be used to
encapsulate an algorithm/main flow with its stepsin a way to achieve steps customization and shared code re-usability. It is achieved 
by creating an abstract class that contains the algorithm definition/main flow with shared code, and the child classes extending the abstract class can customize the steps of the algorithm.

Template Design Pattern suggests that:

1. Break down the algorithm into a series of methods
2. Put a series of calls to these methods or steps inside a single @template method"
3. The steps may either be abstract, or have some defaults implementation inside the parent class
4. To use the algorithm, the client must provide its own subclass and implement all abstract steps

_Video: https://www.youtube.com/watch?v=cGoVDzHvD4A&ab_channel=Geekific_

**<p align="center">Template Method Abstract Class</p>**

~~~java
public abstract class HouseTemplate {

	// Template method, final so subclasses can't override
	public final void buildHouse(){
		buildFoundation();
		buildPillars();
		buildWalls();
		buildWindows();
		System.out.println("House is built.");
	}

	// Default implementation - Chose to override or not
	private void buildWindows() {
		System.out.println("Building Glass Windows");
	}

	private void buildFoundation() {
		System.out.println("Building foundation with cement,iron rods and sand");
	}
	
	// Methods to be implemented by subclasses
	public abstract void buildWalls();
	public abstract void buildPillars();
}
~~~ 

**<p align="center">Template Method Concrete Class</p>**

~~~java
public class WoodenHouse extends HouseTemplate {

	@Override
	public void buildWalls() {
		System.out.println("Building Wooden Walls");
	}

	@Override
	public void buildPillars() {
		System.out.println("Building Pillars with Wood coating");
	}

}
~~~

_Source: https://www.digitalocean.com/community/tutorials/template-method-design-pattern-in-java_

#### What is the JDBC Template?

Spring JdbcTemplate is a powerful mechanism to connect to the database and execute SQL queries. It internally uses JDBC api, but eliminates a lot of problems of JDBC API.

Problems of JDBC API

- We need to write a lot of code before and after executing the query, such as creating connection, statement, closing resultset, connection etc.
- We need to perform exception handling code on the database logic.
- We need to handle transaction.
- Repetition of all these codes from one to another database logic is a time consuming task.

Advantage of Spring JdbcTemplate
- Spring JdbcTemplate eliminates all the above mentioned problems of JDBC API. It provides you methods to write the queries directly, so it saves a lot of work and time.

~~~java
import org.springframework.jdbc.core.JdbcTemplate;  
  
public class EmployeeDao {  

    private JdbcTemplate jdbcTemplate;  
      
    public void setJdbcTemplate(JdbcTemplate jdbcTemplate) {  
        this.jdbcTemplate = jdbcTemplate;  
    }  
      
    public int saveEmployee(Employee e){  
        String query="insert into employee values(  
        '"+e.getId()+"','"+e.getName()+"','"+e.getSalary()+"')";  
        return jdbcTemplate.update(query);  
    }  
    
    public int updateEmployee(Employee e){  
        String query="update employee set   
        name='"+e.getName()+"',salary='"+e.getSalary()+"' where id='"+e.getId()+"' ";  
        return jdbcTemplate.update(query);  
    }  
    
    public int deleteEmployee(Employee e){  
        String query="delete from employee where id='"+e.getId()+"' ";  
        return jdbcTemplate.update(query);  
    }  
    
    public List<Employee> getEmloyees(){  
      String SQL = "select * from Employee";
      List <Employee> employee = jdbcTemplateObject.query(SQL, new EmployeeMapper());
      return students; 
    }  
}
~~~

_Source: https://www.javatpoint.com/spring-JdbcTemplate-tutorial_


<br>

***

## 4. What is a callback? What are the three JdbcTemplate callback interfaces that can be used with queries? What is each used for?

_(You would not have to remember the interface names in the exam, but you should know what they do if you see them in a code sample)._

In Java, a callback is a mechanism that allows a piece of code to be executed at a later time in response to a specific event or condition.

A callback function or method is a reference to executable code that is passed as an argument to another function or method. The function that receives the callback can then call the callback function at the appropriate time, passing any necessary data as arguments.

**On Java level callback can be:**
- Class that implements interface
- Anonymous class
- Lambda expression – JDK 8
- Reference Method – JDK 8

### Jdbc Template Callbacks that can be used with queries:

**RowMapper**

In the JDBC template of Spring Framework, `RowMapper` is an interface that defines a strategy for mapping a row of a `ResultSet` to an object.

When we execute a query using JDBC template, we get a `ResultSet` that contains the results of the query. We need to map this `ResultSet` to a collection of objects that can be used in our application. The `RowMapper` interface provides a way to map each row of the `ResultSet` to an object.

The `RowMapper` interface has one method: `mapRow()`. This method takes two arguments: the `ResultSet` object and the row number. It returns the mapped object.

For example, let's say we have a database table called employees with columns id, name, and age. We can use a `RowMapper` to map each row of the `ResultSet` to an `Employee` object:

```java
public class EmployeeRowMapper implements RowMapper<Employee> {

    @Override
    public Employee mapRow(ResultSet rs, int rowNum) throws SQLException {
        Employee employee = new Employee();
        employee.setId(rs.getInt("id"));
        employee.setName(rs.getString("name"));
        employee.setAge(rs.getInt("age"));
        return employee;
    }
}
```

In this example, we've created a **RowMapper** implementation called `EmployeeRowMapper` that maps each row of the `ResultSet` to an `Employee` object. We use the `ResultSet` to get the values of each column for the current row, and then create a new `Employee` object with those values.

We can then use this `RowMapper` to execute a query and map the results to a list of `Employee` objects:

```java
List<Employee> employees = jdbcTemplate.query(
        "SELECT id, name, age FROM employees",
        new EmployeeRowMapper());

```
In this example, we're using the `query()` method of `JdbcTemplate` to execute a query and map the results to a list of `Employee` objects using the `EmployeeRowMapper`. The `query()` method executes the SQL query and returns the results as a list of `Employee` objects.

<br>

**RowCallbackHandler**

In JDBC template of Spring Framework, `RowCallbackHandler` is an interface that allows us to process each row of a `ResultSet` object individually, without the need to map the entire result set to a collection of objects.

When we execute a query using JDBC template, we can use a `RowCallbackHandler` to process each row of the `ResultSet` one at a time. This can be useful when we want to perform some operation on each row individually, such as printing the values or updating some other data based on the values.

The `RowCallbackHandler` interface has one method: `processRow()`. This method takes two arguments: the `ResultSet` object and the row number.

For example, let's say we have a database table called employees with columns `id`, `name`, and `age`. We can use a `RowCallbackHandler` to print the values of each row:

```java
public class EmployeeRowCallbackHandler implements RowCallbackHandler {

    @Override
    public void processRow(ResultSet rs) throws SQLException {
        int id = rs.getInt("id");
        String name = rs.getString("name");
        int age = rs.getInt("age");
        System.out.println("Employee: id=" + id + ", name=" + name + ", age=" + age);
    }
}
```

In this example, we've created a `RowCallbackHandler` implementation called `EmployeeRowCallbackHandler` that prints the values of each row of the `ResultSet`. We use the `ResultSet` to get the values of each column for the current row and then print them.

We can then use this `RowCallbackHandler` to execute a query and process the results:

```java
jdbcTemplate.query("SELECT id, name, age FROM employees", new EmployeeRowCallbackHandler());
```

In this example, we're using the `query()` method of `JdbcTemplate` to execute a query and process the results using the `EmployeeRowCallbackHandler`. The `query()` method executes the SQL query and calls the `processRow()` method of the `EmployeeRowCallbackHandler` for each row in the `ResultSet`.


<br>

***

## 5. Can you execute a plain SQL statement with the JDBC template?


Yes, the JDBC template of Spring Framework provides methods to execute plain SQL statements.

The `execute()` method of `JdbcTemplate` is used to execute any SQL statement and it returns a `Boolean` value indicating whether the statement was successfully executed or not.

Here's an example of how to use the `execute()` method to execute a plain SQL statement:

```java
jdbcTemplate.execute("CREATE TABLE employees (id INT PRIMARY KEY, name VARCHAR(50), age INT)");
```

In this example, we're using the `execute()` method of `JdbcTemplate` to create a new table named employees with three columns: `id`, `name`, and `age`. The `execute()` method executes the SQL statement and returns a `Boolean` value indicating whether the statement was successfully executed or not.

We can also use the `update()` method of `JdbcTemplate` to execute plain SQL statements that update the database. The `update()` method returns the number of rows affected by the statement.

Here's an example of how to use the `update()` method to update data in a table:

```java
jdbcTemplate.update("UPDATE employees SET age = 30 WHERE name = 'John'");
```
In this example, we're using the `update()` method of `JdbcTemplate` to update the age column of the employees table for all rows where the name column equals 'John'. The `update()` method executes the SQL statement and returns the number of rows affected by the statement.
