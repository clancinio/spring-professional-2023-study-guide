# Spring Certified Professional 2023 - Study Guide

## Module 3 - Data Management: JDBC, Transactions, Spring Data JPA

***

### 1. What is the difference between checked and unchecked exceptions?

#### Checked Exceptions

In general, checked exceptions represent errors outside the control of the program. For example, the constructor of FileInputStream throws FileNotFoundException if the input file does not exist.

**Java verifies checked exceptions at compile-time.**

Therefore, we should use the throws keyword to declare a checked exception:

```
private static void checkedExceptionWithThrows() throws FileNotFoundException {
    File file = new File("not_existing_file.txt");
    FileInputStream stream = new FileInputStream(file);
}
```
We can also use a try-catch block to handle a checked exception:


```
private static void checkedExceptionWithThrows() throws FileNotFoundException {
    File file = new File("not_existing_file.txt");
    FileInputStream stream = new FileInputStream(file);
}
```
Some common checked exceptions in Java are IOException, SQLException and ParseException.

The Exception class is the superclass of checked exceptions, so we can create a custom checked exception by extending Exception:

```
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

```
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

### 2. How do you configure a DataSource in Spring? Which bean is very useful for development/test databases?

Configuration of Data Source in Spring is dependent on the type of application that is executed.

#### Standalone

Data Source is configured in @Configuration class and is created as a bean of one of the supported data source types.

~~~
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


***

### 3. What is the Template design pattern and what is the JDBC template?


