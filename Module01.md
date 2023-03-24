# Spring Certified Professional 2023 - Study Guide

## Module 1 - Container, Dependency, and IOC

***

### 1. What is dependency injection and what are the advantages?
Dependency injection is a technique of creating software in which objects 
do not create their own dependencies. Instead, objects declare dependencies that they need
and it is the external objects job, or framework, to provide concrete dependencies to the object.

Types of Dependency Injection:
- Constructor injection
- Setter injection
- Interface injection

Advantages of using Dependency Injection:
- Increases code re-usability  
- Increases code readability 
- Increases code maintainability 
- Increases code testability
- Reduces coupling
- Increases cohesion 

#### Example

Without dependency injection - tightly coupled
- It cannot be used and tested independently from this class (hard-dependency)

```java
public class Chef {

    private Food burger;
    
    public Chef() {
        burger = new Burger();
    }
    
    public void cook() {
        // code...
    }
}
```
With dependency injection - loosely coupled
- We decoupled the construction of the main class from the construction of it's dependencies
- The dependency is provided from the outside
- If the objects implementation changes in the future,  it is no longer the dependent classes responsibility to figure out what actually changed

```java
public class Chef {

    private Food food;
    
    public Chef(Food food) {
        this.food = food;
    }
    
    public void cook() {
        // code...
    }
}
```

***

### 2. What is a pattern? What is an anti-pattern? Is dependency injection a pattern?

A Software Design Pattern is a reusable solution to often, commonly occurring problem in software design. It is a high level description on how to solve the problem that can be used in many different situations. Design patterns often represent best practices that developers can use to solve common problems.

Commonly used design patterns: 

- Builder
- Factory Method
- Template Method
- Visitor
- Facade
- Composite
- Strategy 
- Observer

An anti-pattern is an ineffective and counter-productive solution to an often occurring problem.

Examples of anti-patterns in OOP:

- God Object
- Circular dependency 
- Constant interface 
- Sequential coupling

Dependency injection is a pattern that solves the problem of flexible dependency creation.

***

### 3. What is an interface and what are the advantages of making use of them in Java? Why are they recommended for Spring Beans?

**OOP Definition** 

An interface is a description of the actions an object can perform. It is a way to enforce actions on objects that implement the interface.

**Java Definition** 

An Interface in Java programming language is defined as an abstract type used to specify the behavior of a class. An interface in Java is a blueprint of a behaviour. A Java interface contains static constants and abstract methods.

The interface in Java is a mechanism to achieve abstraction. There can be only abstract methods in the Java interface, not the method body. It is used to achieve abstraction and multiple inheritance in Java. In other words, you can say that interfaces can have abstract methods and variables. It cannot have a method body. Java Interface also represents the IS-A relationship.

When we decide a type of entity by its behaviour and not via attribute we should define it as an interface.

Like a class, an interface can have methods and variables, but the methods declared in an interface are by default abstract (only method signature, no body).

A Java interface may also contain:

- Constants 
- Default methods (Java 8)
- Static methods
- Nested types

#### Advantages of using interfaces in Java 

- Allows decoupling between the contract and it's implementation 
- Allows declaring contract between callee and caller
- Increases interchangeability 
- Increases testability 

#### Advantages of using interfaces in Spring

- Allows for use of JDK Dynamic proxy 
- Allows implementation hiding 
- Allows to easily switch beans

<br />

<p align="center">Vehicle Interface</p>

~~~java
interface Vehicle {
     
    // all are the abstract methods.
    void changeGear(int a);
    void speedUp(int a);
    void applyBrakes(int a);
}
~~~

<p align="center">Vehicle Bicycle Class</p>

```java
class Bicycle implements Vehicle{
     
    int speed;
    int gear;
     
    // to change gear
    @Override
    public void changeGear(int newGear){
         
        gear = newGear;
    }
     
    // to increase speed
    @Override
    public void speedUp(int increment){
         
        speed = speed + increment;
    }
     
    // to decrease speed
    @Override
    public void applyBrakes(int decrement){
         
        speed = speed - decrement;
    }
     
    public void printStates() {
        System.out.println("speed: " + speed
            + " gear: " + gear);
    }
}
```

_Source: https://www.geeksforgeeks.org/interfaces-in-java/_
***

### 4. What is meant by "application-context"?

The Application Context is a central part of a Spring Application. It holds bean definitions and contains registry and application components. It allows you to retrieve assembled and configured beans.

**Main responsibilities of Application Context:** 

- Initiates, configures, and assembles beans
- Manages bean lifecycle
- Is a Bean Factory
- Is a Resource Loader
- Has the ability to push events to registered event listeners 
- Exposes environment which allows to resolve properties

**Common Application Context types:**

- AnnotationConfigApplicationContext
- AnnotationConfigWebApplicationContext
- ClassPathXmlApplicationContext
- FileSystemXmlApplicationContext
- XmlWebApplicationContext



***

### 5. What is the concept of a "container" and what is its lifecycle?

The Spring container is the core of Spring Framework. The container is used for creating the objects and configuring them. Also, Spring IoC Containers use for managing the complete lifecycle from creation to its destruction. It uses Dependency Injection (DI) to manage components and these objects are called Spring Beans. The container uses configuration metadata which represent by Java code, annotations or XML along with Java POJO classes

**Spring Container Lifecycle**

- Application is started
- Spring container is created
- Container reads configuration
- Bean definitions are created from configuration
- BeanFactoryPostProcessors are processing bean definitions
- Instances of Spring Beans are created
- Spring Beans are configured and assembled - resolve property values and inject
- BeanPostProcessors are called
- Application runs
- Application gets shut down
- Spring context is closed
- Destruction callbacks are invoked

***

### 6. How are you going to create a new instance of an ApplicationContext?

Non-Web Application:

- AnnotationConfigApplicationContext
- ClassPathXmlApplicationContext
- FileSystemXmlApplicationContext 

Web Applications:

- Servlet 2 - web.xml, ContextLoaderListener, DispatchServlet
- Servlet 3 - XmlWebApplicationContext
- Servlet 3 - AnnotationConfigWebApplicationContext

Spring Boot:

- SpringBootConsoleApplication - CommandLineRunner
- SpringBootApplication - Embedded Tomcat



***

### 7. Can you describe the lifecycle of a Spring Bean in an ApplicationContext?

**Context is created**

- Bean definitions are created based on Spring Bean Configuration
- BeanFactoryPostProcessors are invoked

**Bean is created**

- Instance of Bean is created
- Properties and Dependencies are set
- BeanPostProcessor::postProcessBeforeInitialization is called
- @PostConstruct method gets called
- InitializingBean::afterPropertiesSet method gets called
- @Bean(initMethod) gets called
- BeanPostProcessor::postProcessAfterInitialization gets called

Bean is ready to use

**Bean is destroyed**

- @PreDestroy method gets called
- DisposableBean::destroy gets called 
- @Bean(destroyMethod) method gets called 


***

### 8. How are you going to create an ApplicationContext in an Integration test? 

Add Spring test dependency:

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-test</artifactId>
    <version>5.1.6.RELEASE</version>
    <scope>test</scope>
</dependency>
```

Add Spring Runner to the test class:

```java
@RunWith(SpringRunner.class)
public class ServiceTest {
    // ...
}
```

Add Context Configuration to the test class

```java
@RunWith(SpringRunner.class)
@ContextConfiguration(classes = AppConfig.class) 
public class ServiceTest {
    // ...
}
```

***

### 9. What is the preferred way to close an Application Context? Does Spring do this for you?

Standalone non-web applications:

- Register shutdown hook by calling ConfigurableApplicationContext#registerShutdownHook - recommended way
- ConfigurableApplicationContext#close

Web Application:

- ContextLoaderListener will automatically close context when the web containers stops the web application

Spring Boot:

- Application Context will automatically be closed 
- Shutdown hook will automatically be registered 
- ContextLoaderListener applies to Spring Boot Web Application as well 

***

### 10. Can you describe: Dependency Injection using Java configuration? DI using annotations (@Component, @Autowired)? Component scanning, Stereotypes and MetaAnnotations? Scopes for Spring Beans? What is the default scope?

**Dependency Injection using Java configuration**

When using DI with Java configuration, you need to explicitly define all of your beans and use @_Bean_ (to define the bean) and @_Autowire_ (to inject any dependencies) at the method level.

Example:

```java
@Configuration
public class AppConfiguration {

    @Bean
    @Autowired
    public Bean1 bean1(Bean2 bean2, Bean3 bean3){
        return  Bean1(bean2, bean3);
    }
    
    @Bean
    public Bean2 bean2(){
        return new Bean2();
    }
    
    @Bean
    public Bean3 bean3(){
        return new Bean3();
    }
}
```
<br> 

**Dependency Injection using Annotations**

Create classes annotated with the @Component annotation:

```java
@Component
public class SpringBeanOne {}
```
```java
@Component
public class SpringBeanTwo {}
```
```java
@Component
public class SpringBeanThree {}
```
<br> 
Define dependencies when required:

```java
@Component
public class SpringBeanOne {

    @Autowired
    public SpringBeanTwo springBeanTwo;
    
    @Autowired
    public SpringBeanThree springBeanThree;
}
```
<br>

Create configuration with Component Scanning enabled:

```java
@ComponentScan
public class ApplicationConfiguration {}
```
<br> 

**Component Scanning**

Component Scanning is the process in which Spring scans the Classpath in search for classes annotated with stereotype annotations  (@Component, @Service, @Controller...) and based on those create bean definitions.

Simple Component Scanning within Configuration package and all subclasses:
```
@ComponentScan
public class ApplicationConfiguration {
}
```

Advanced Component Scanning rules:

```java
@ComponentScan(
        basePackages = "com.spring.professional.exam.tutorial.module01.question10.annotations.beans",
        //basePackageClasses = SpringBean1.class,
        includeFilters = @ComponentScan.Filter(type = FilterType.REGEX, pattern = ".*Bean.*"),
        excludeFilters = @ComponentScan.Filter(type = FilterType.REGEX, pattern = ".*Bean1.*")
)
public class ApplicationConfigurationAdvanced {
}
```

<br> 

**Stereotypes - Definition**

Stereotypes are annotations applied to classes to describe the role that will be performed by this class. Spring discovers classes by stereotypes and creates definitions based on those types.

![](images/annotations.png)

Types of Stereotypes:

- Component - generic component in the system, root stereotype, candidate for autoscanning
- Service - class will contain business logic
- Repository - class is a data repository used for data access objects and persistence
- Controller - class is a controller, usually a web controller (used with @RequestMapping)

_Source: https://www.udemy.com/course/spring-certified-tutorial/_

<br> 

**Meta-annotations**

Meta-annotations are annotations that can be used to create new annotations.

Example:

@RestController annotation is using @Controller

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Controller
@ResponseBody
public @interface RestController {

@AliasFor(annotation = Controller.class)
String value() default "";

}
```

<br> 

**Scopes of Spring Beans**

| Scope      | Description                                |
|------------|--------------------------------------------|
| Singleton  | Single Bean per Spring Container (Default) |
| Prototype  | New instance each time a Bean is requested |
| Request    | New instance per each HTTP Request         |
| Session    | New instance per each HTTP Session         |
| Application | One instance per each ServletContext       |
| Websocket  | One instance per each WebSocket            |


_Source: https://www.udemy.com/course/spring-certified-tutorial/_


***

### 11. Are beans lazily or eagerly instantiated by default? How do you alter this behaviour?

**Lazy and Eager Instance Creation vs Scope Type**

- Singleton Beans are eagerly instantiated by default

- Prototype Beans are lazily instantiated by default (instance is created when a bean is requested)

- …however, if Singleton Bean has a dependency on Prototype Bean, then Prototype Bean Instance will be created eagerly to satisfy dependencies for Singleton Bean
So whenever you have a lazy or eagerly instantiation by default it depends on the scope of the bean. If you have a Singleton bean which is default scope then this bean will be eagerly instantiated by default. But if you are working with the prototype bean you will have a lazy instantiation by default.
And there is one exception of this rule. If you have a Singleton bean that has a dependency on the prototype bean then the prototype instance will be created eagerly to satisfy the dependency for a singleton bean.

**Altering Behavior**


1) **You can change the default behavior for all beans by @ComponentScan annotation**

```java
@ComponentScan(lazyInit = true)
@Configuration
public class AppConfig {}
```

- Setting lazyInit to true, will make all beans lazy, even Singleton Beans
- Setting lazyInit to false (default), will create Singleton Beans Eagerly and Prototype Beans Lazily

2) **You can also change the default behavior by using @Lazy annotation**

   - @Lazy annotation takes one parameter - Whether lazy initialization should occur
   -  By default, @Lazy is used to mark a bean as lazily instantiated
   - You can use @Lazy(false) to force Eager Instantiation – use case for @ComponentScan(lazyInit = true) when some beans always need to be instantiated eagerly
   <br>
   
   There are some cases when you want to create all of your beans lazy but one of the beans you want to instantiate eagerly. For example, this bean create some code that initiates the database connection and something like this.
   
   And there is a way to force the eager initialization. You can do it with the @Lazy(false) annotation.


3. **@Lazy can be applied to:**

   - Classes annotated with @Component – makes bean Lazy or as specified by @Lazy parameter. Setting @Lazy to Prototype scope beans doesn't make sense, because prototypes are lazy by default.
   
   ```java
   @Componet
   @Lazy
   public class SpringBean(){
      public SpringBean(){
         System.ou.println("Creating " + getClass().getSimpleName() + " - Lazy Singleton Bean");
      }
   }
   ```
   - Classes annotated with @Configuration annotation – make all beans provided by configuration lazy or as specified by @Lazy parameter
   
   ```java
   @Configuration
   @Lazy
   public class ApplicationConfig(){
   
      public SpringBeanOne(){
         return new SpringBeanOne();
      }
   
      public SpringBeanTwo(){
         return new SpringBeanOne();
      }
   }
   ```
   
   - The method annotated with @Bean annotation – makes bean created by method Lazy or as specified by @Lazy parameter

   ```java
   @Configuration
   public class ApplicationConfig(){
   
      @Lazy
      public SpringBeanOne(){
         // Lazily instantiated
         return new SpringBeanOne();
      }
   
      public SpringBeanTwo(){
         return new SpringBeanOne();
      }
   }
   ```
   ```java
   @Configuration
   @Lazy
   public class ApplicationConfig(){
   
      @Lazy(lazyInit = false)
      public SpringBeanOne(){
         // Eagerly instantiated
         return new SpringBeanOne();
      }
   
      public SpringBeanTwo(){
         return new SpringBeanOne();
      }
   }
   ```

_Source: https://www.udemy.com/course/spring-certified-tutorial/_

***

### 12. What is a property source? How to use @PropertySource?

PropertySource is Spring Abstraction on Environment Key-Value pairs, which can come from:

- JVM Properties
- System Environmental Variables
- JNDI Properties
- Servlet Parameters
- Properties File Located on Filesystem
- Properties File Located on Classpath

It is a good way to access all of the properties with the usage of the same mechanism without having to distinguish whether this property is coming from JVM or from the system.

Remember that, JVM Properties, System Environmental Variables, JNDI Properties, Servlet Parameters are already read for you.

You read properties with the usage of @PropertySource or @PropertySources annotation:

```java
@ComponentScan
@PropertySources({
   @PropertySource("file: ${app-home}/app-db.properties"),
   @PropertySource("classPath: app-defaults/.properties")
})
public class AppConfig{
}
```

You access properties with the usage of @Value annotation:

```java
@Component
public class SpringBeanOne{

   @Value("db.host")
   private String dbHost;
   
   @Value("app.envId")
   private String appEnvId;
   
   @Value("external.service")
   private String externalService;
   
   @Value("JAVA_HOME") // Defined at system level
   private String javaHome;
   
   public void printProps() {
   }
   
}
```
_Source: https://www.udemy.com/course/spring-certified-tutorial/_

***

### 13. What is a BeanFactoryPostProcessor and PropertySourcesPlaceholderConfigurer?

#### What is a BeanFactoryPostProcessor and what is it used for?

BeanFactoryPostProcessor is an interface that contains single method postProcessBeanFactory, implementing it allows you to create logic that will modify Spring Bean Metadata before any Bean is created. BeanFactoryPostProcessor does not create any beans, however, it can access and alter Metadata that is used later to create Beans.

#### When is BeanFactoryPostProcessor invoked?


BeanFactoryPostProcessor is invoked after Spring will read or discover Bean Definitions, but before any Spring Bean is created.

#### Why would you define a static @Bean method?

Because BeanFactoryPostProcessor is also a Spring Bean, but a special kind of Bean that should be invoked before other types of beans get created, Spring needs to have the ability to create it before any other beans. This is why BeanFactoryPostProcessors needs to be registered from a static method level.

**Let's look in the code!**

Here we have CustomBeanFactoryPostProcessor and it implements BeanFactoryPostProcessor. The method _postProcessBeanFactory_ prints out the beans in the package.

```java
public class CustomBeanFactoryPostProcessor implements BeanFactoryPostProcessor {
    @Override
    public void postProcessBeanFactory(ConfigurableListableBeanFactory beanFactory) throws BeansException {
        stream(beanFactory.getBeanDefinitionNames())
                .map(beanFactory::getBeanDefinition)
                .filter(beanDefinition -> beanClassNameContains(beanDefinition, "module01.question13.beans"))
                .map(BeanDefinition::getBeanClassName)
                .forEach(System.out::println);
    }

    private boolean beanClassNameContains(BeanDefinition beanDefinition, String subString) {
        return beanDefinition.getBeanClassName() != null && beanDefinition.getBeanClassName().contains(subString);
    }
}
```
<br>

To register this class, we are utilizing the application configuration with a static method annotated with @Bean, which instructs Spring to create the bean before any other bean:

```java
@ComponentScan
@PropertySource("classpath:/app-defaults.properties")
public class ApplicationConfiguration {
   @Bean
   public static CustomBeanFactoryPostProcessor customerBeanFactoryPostProcessor() {
      return new CustomBeanFactoryPostProcessor();
   }
}
```
<br>

#### What is a PropertySourcesPlaceholderConfigurer used for?

Spring has already implemented `PropertySourcesPlaceholderConfigurer` as a `BeanFactoryPostProcessor`. This class is called before any object creation and its purpose is to resolve property placeholders in Spring Beans that are annotated with `@Value("${property_name}").`

_Source: https://www.udemy.com/course/spring-certified-tutorial/_

***

### 14. What is a BeanPostProcessor and how is it different to a BeanFactoryPostProcessor? What do they do?

###### What is a BeanPostProcessor?

BeanPostProcessor is an interface that allows you to create extensions to Spring Framework that will modify Spring Beans objects during initialization. This interface contains two methods:
- `postProcessBeforeInitialization`
- `postProcessAfterInitialization`

Implementing those methods allows you to modify created and assembled bean objects or even switch object that will represent the bean.


######  How is it different from a BeanFactoryPostProcessor?

The main difference compared to BeanFactoryPostProcessor is that BeanFactoryPostProcessor works with Bean Definitions while BeanPostProcessor works with Bean Objects.

And it also important to remember that when methods of BeanPostProcessor will get called, you have bean object is already created and also it is assembled. That means that all of the properties and all of the dependencies are already set.

###### What do they do? When are they called?

BeanFactoryPostProcessor and BeanPostProcessor in Spring Container Lifecycle:

1. Beans Definitions are created based on Spring Bean Configuration

2. `BeanFactoryPostProcessors` are invoked

3. Instance of Bean is Created

4. Properties and Dependencies are set

5. `BeanPostProcessor::postProcessBeforeInitialization` gets called

6. @PostConstruct method gets called

7. `InitializingBean::afterPropertiesSet` method gets called

8. `@Bean(initMethod) `method gets called

9. `BeanPostProcessor::postProcessAfterInitialization` gets called

The recommended way to define BeanPostProcessor is through static @Bean method in Application Configuration. This is because BeanPostProcessor should be created early before other Beans Objects are ready.

```java
@ComponentScan
public class ApplicationConfiguration {
  
    @Bean
    public static CustomBeanPostProcessor customBeanPostProcessor() {
        return new CustomBeanPostProcessor();
    }
    
    // Other Beans ...
}

```
It is also possible to create BeanPostProcessor through regular registration in Application Configuration or through Component Scanning and @Component annotation, however, because in that case bean can be created late in processes, recommended way is options provided above.

Here is a CustomBeanPostProcessor and it doesn't modify any bean, just print out their names. But it is possible to modify the bean object here.

As you can see bean object is passed into each method and returned. That means that I cannot only change this bean but also I can switch it or create a proxy object for this bean.

###### What is an initialization method and how is it declared on a Spring bean?

**Initialization method** is a method that you can write for Spring Bean if you need to perform some initialization code that depends on properties and/or dependencies injected into Spring Bean.

_You might be thinking about why we cannot put this one in the constructor? The answer for this is that when constructor would get called, there is no guarantee that all of the dependencies will be already set up. This is might be the case when we are using private field injection or property injection through the setter instead of using constructor injection._

You can declare the Initialization method in three ways:

- Create a method in Spring Bean annotated with `@PostConstruct`:

```java
public class SpringBean1 {

    @PostConstruct
    public void postConstruct() {
        System.out.println(getClass().getSimpleName() + "::postConstruct");
    }
}
```
Implement `InitializingBean::afterPropertiesSet`:

```java
public class SpringBean1 implements InitializingBean {

   @Override
   public void afterPropertiesSet() throws Exception {
      System.out.println(getClass().getSimpleName() + "::afterPropertiesSet");
   }
}
```
Create Bean in Configuration class with @Bean method and use @Bean(initMethod)

```java
public class SpringBean1 {

   public void initMethod() {
      System.out.println(getClass().getSimpleName() + "::initMethod");
   }
}
```

```java
@ComponentScan
public class ApplicationConfiguration {
  
   @Bean
   public static CustomBeanPostProcessor customBeanPostProcessor() {
      return new CustomBeanPostProcessor();
   }

   @Bean(initMethod = "initMethod")
   public SpringBean1 springBean1() {
      return new SpringBean1();
   }
}
```

###### What is a destroy method, how is it declared?

**Destroy method** is a method in Spring Bean that you can use to implement any cleanup logic for resources used by the Bean. The method will be called when Spring Bean will be taken out of use, this is usually happening when the Spring Context is closed.

_Let's say that you have a bean and it keeps a state. Inside of the bean you are keeping connections to the database. When the application is stopped and the Spring context is getting closed you would most probably like to close all of those DB connections. So this is a great place to implement this logic._

You can declare destroy method in the following ways:

- Create method annotated with `@PreDestroy` annotation

```java
public class SpringBean2 {

    @PreDestroy
    public void preDestroy() {
        System.out.println(getClass().getSimpleName() + "::preDestroy");
    }
}
```

- Implement `DisposableBean::destroy`:

```java
public class SpringBean2 implements DisposableBean {
  
   @Override
   public void destroy() throws Exception {
      System.out.println(getClass().getSimpleName() + "::destroy");
   }
}
```
- Create Bean in Configuration class with `@Bean` method and use `@Bean(destroyMethod)`

```java
public class SpringBean2 implements DisposableBean {
  
    public void destroyMethod() {
        System.out.println(getClass().getSimpleName() + "::destroyMethod");
    }
}
```

```java
@ComponentScan
public class ApplicationConfiguration {
  
    @Bean
    public static CustomBeanPostProcessor customBeanPostProcessor() {
        return new CustomBeanPostProcessor();
    }

    @Bean(initMethod = "initMethod")
    public SpringBean1 springBean1() {
        return new SpringBean1();
    }

    @Bean(destroyMethod = "destroyMethod")
    public SpringBean2 springBean2() {
        return new SpringBean2();
    }
}
```
<br>

###### Consider how you enable JSR-250 annotations like @PostConstruct and @PreDestroy?

When using AnnotationConfigApplicationContext support for @PostConstruct and @PreDestroy is added automatically.

Those annotations are handled by CommonAnnotationBeanPostProcessor with is automatically registered by AnnotationConfigApplicationContext.

**When/how will they (initialization, destroy methods) get called?**

**Context is Created:**

1. Beans Definitions are created based on Spring Bean Configuration

2. BeanFactoryPostProcessors are invoked

**Bean is Created:**

1. Instance of Bean is Created

2. Properties and Dependencies are set

3. `BeanPostProcessor::postProcessBeforeInitialization` gets called

4. `@PostConstruct` method gets called

5. `InitializingBean::afterPropertiesSet` method gets called

6. `@Bean(initMethod)` method gets called

7. `BeanPostProcessor::postProcessAfterInitialization` gets called

**Bean is Ready to use**

Bean is Destroyed (usually when the context is closed):

1. `@PreDestroy` method gets called
2. `DisposableBean::destroy` method gets called
3. `@Bean(destroyMethod)` method gets called


***

### 15. What does component-scanning do?

#### Component Scanning

Process in which Spring is scanning Classpath in search for classes annotated with stereotypes annotations (@Component, @Repository, @Service, @Controller, …) and based on those creates beans definitions.

**Simple component scanning** within Configuration package and all sub-packages:
```java
@ComponentScan
public class ApplicationConfiguration {
}
```
**Advanced Component Scanning** Rules:
```java
@ComponentScan(
        basePackages = "com.spring.professional.exam.tutorial.module01.question15.advanced.beans",
        includeFilters = @ComponentScan.Filter(type = FilterType.REGEX, pattern = ".*Bean"),
        excludeFilters = @ComponentScan.Filter(type = FilterType.REGEX, pattern = ".*(Controller|Service).*")
)
public class ApplicationConfiguration {
}
```
In the above example, component scanning will search for beans in the package. And beans with the name ends with "Bean" will be included. And beans that contain "Controller" or "Service" in their names will be excluded.


***

### 16. What is the behaviour of the annotation @Autowired?

What is the `@Autowired `annotation?

`@Autowired` is an annotation that is processed by AutowiredAnnotationBeanPostProcessor, which can be put onto class constructor, field, setter method or config method. Using this annotation enables automatic Spring Dependency Resolution that is primarily based on types.

`@Autowired `has a property required which can be used to tell Spring if dependency is required or optional. By default, the dependency is required. If @Autowired with required dependency is used on top of constructor or method that contains multiple arguments, then all arguments are considered required dependency unless the argument is of type Optional, is marked as @Nullable, or is marked as @Autowired(required = false).

If _@Autowired_ is used on top of Collection or Map then Spring will inject all beans matching the type into Collection and key-value pairs as BeanName-Bean into Map. Order of elements depends on the usage of @Order, @Priority annotations and implementation of Ordered interface.

**@Autowired uses the following steps when resolving dependency:**

- Match exactly by type, if only one found, finish

- If multiple beans of the same type found, check if any contains @Primary annotation, if yes, inject @Primary bean and finish

- If no exactly one match exists, check if @Qualifier exists for the field, if yes use @Qualifier to find matching bean

- If still no exactly one bean found, narrow the search by using a bean name

- If still no exactly one bean found, throw an exception (NoSuchBeanDefinitionException, NoUniqueBeanDefinitionException, …)

<br> 

###### @Autowired with field injection

```java
@Service
public class RecordsService01 {

   @Autowired
   public DbRecordsReader recordsReader;

   @Autowired
   protected DbRecordsBackup recordsBackup;

   @Autowired
   private DbRecordsProcessor recordsProcessor;

   @Autowired
   DbRecordsWriter recordsWriter;

   @Autowired
   private Optional<RecordsHash> recordsHash;

   @Autowired
   @Nullable
   private RecordsUtil recordsUtil;

   @Autowired(required = false)
   private RecordsValidator recordsValidator;

   //...
}
```

- Autowired fields can have any visibility level
- The injection is happening after Bean is created but before any init method (@PostConstruct, InitializingBean, @Bean(initMethod)) is called
- By default field is required, however, you can use Optional, @Nullable or @Autowired(required = false) to indicate that field is not required.


After running this example, some of the beans are empty. This is because those are marked as Optional

```
RecordsService01 recordsHash = Optional.empty
RecordsService01 recordsUtil = null
RecordsService01 recordsValidator = null
```
<br>

###### @Autowired with constructor

The constructor can have any access modifier (public, protected, private, package-private).

If there is only one constructor in a class, there is no need to use `@Autowired` on top of it, Spring will use this default constructor anyway and will inject dependencies into it.

If a class defines multiple constructors, then you are obligated to use `@Autowired `to tell Spring which constructor should be used to create Spring Bean. If you will have a class with multiple constructors without any of constructor marked as `@Autowired` then Spring will throw `NoSuchMethodException`.

By default all arguments in constructor are required, however, you can use `Optional`, `@Nullable` or `@Autowired(required = false)` to indicate that parameter is not required.

```java
@Service
public class RecordsService02 {
   public RecordsService02(DbRecordsReader recordsReader, DbRecordsBackup recordsBackup, DbRecordsProcessor recordsProcessor, DbRecordsWriter recordsWriter) {
      System.out.println(
              getClass().getSimpleName() + " recordsReader = " + recordsReader + "\n" +
                      getClass().getSimpleName() + " recordsBackup = " + recordsBackup + "\n" +
                      getClass().getSimpleName() + " recordsProcessor = " + recordsProcessor + "\n" +
                      getClass().getSimpleName() + " recordsWriter = " + recordsWriter + "\n"
      );
   }
}
```

In this example, there is only one constructor. And there is no need to use `@Autowired`. Spring already knows which dependencies need to be injected. All dependencies will be resolved.

In the next example, one of the constructors is annotated with `@Autowired`. And the bean will be created correctly.

Also, some of the dependencies marked as Optional and they will be empty or null. But constructor will be still called correctly.

```java
@Service
public class RecordsService03 {

   @Autowired
   private RecordsService03(DbRecordsReader recordsReader, DbRecordsProcessor recordsProcessor, Optional<RecordsUtil> recordsUtil, @Nullable RecordsHash recordsHash, @Autowired(required = false) RecordsValidator recordsValidator) {
      System.out.println(
              getClass().getSimpleName() + " recordsReader = " + recordsReader + "\n" +
                      getClass().getSimpleName() + " recordsProcessor = " + recordsProcessor + "\n" +
                      getClass().getSimpleName() + " recordsUtil = " + recordsUtil + "\n" +
                      getClass().getSimpleName() + " recordsHash = " + recordsHash + "\n" +
                      getClass().getSimpleName() + " recordsValidator = " + recordsValidator + "\n"
      );
   }

   //@Autowired
   RecordsService03(DbRecordsReader recordsReader, DbRecordsProcessor recordsProcessor) {
      System.out.println(
              getClass().getSimpleName() + " recordsReader = " + recordsReader + "\n" +
                      getClass().getSimpleName() + " recordsProcessor = " + recordsProcessor + "\n"
      );
   }
}
```
<br>

###### @Autowired with the method injection

```java
@Service
public class RecordsService04 {

   @Autowired
   public void setRecordsReader(DbRecordsReader recordsReader) {
      System.out.println(
              getClass().getSimpleName() + " setRecordsReader:\n" +
                      "\trecordsReader = " + recordsReader + "\n"
      );
   }
   
   // ...
}
```

`@Autowired` method can have any visibility level and also can contain multiple parameters.

If the method contains multiple parameters, then by default it is assumed that in `@Autowired `method all parameters are required. If Spring will be unable to resolve all dependencies for this method, `NoSuchBeanDefinitionException` or `NoUniqueBeanDefinitionException` will be thrown.

When using `@Autowired(required = false) `with the method, it will be invoked only if Spring can resolve all parameters.

```java
@Autowired(required = false)
public void setRecordsReaderAndRecordsValidator(DbRecordsReader recordsReader, RecordsValidator recordsValidator){
        System.out.println(
        getClass().getSimpleName()+" setRecordsReaderAndRecordsValidator:\n"+
        "\trecordsReader = "+recordsReader+"\n"+
        "\trecordsValidator = "+recordsValidator+"\n"
        );
}
```

If you want Spring to invoke method only with arguments partially resolved, you need to use @Autowired method with parameter marked as Optional, @Nullable or `@Autowired(required = false)` to indicate that this parameter is not required.

```java
@Autowired
public void setRecordsReaderAndRecordsValidator(DbRecordsReader recordsReader, @Nullable RecordsValidator recordsValidator) {
        System.out.println(
        getClass().getSimpleName() + " setRecordsReaderAndRecordsValidator:\n" +
        "\trecordsReader = " + recordsReader + "\n" +
        "\trecordsValidator = " + recordsValidator + "\n"
        );
        }
```
<br>

###### @Autowired with Collections

RecordsReader is an interface that is implemented by 4 classes. And we can inject all of these Beans into a List.

```java
@Service
public class RecordsService05 {

    @Autowired
    public void setRecordsReaders(List<RecordsReader> recordsReaders) {
        System.out.println(getClass().getSimpleName() + " setRecordsReaders:");
        recordsReaders.stream()
                .map(r -> "\t" + r.getClass().getSimpleName())
                .forEach(System.out::println);
    }
}
```

After running this example, we can see the next output:

```
RecordsService05 setRecordsReaders:
   SocketRecordsReader
   WebServiceRecordsReader
   FileRecordsReader
   DbRecordsReader
```

Because Spring injects all beans with a type of RecordReader to list. And the order is not random. It is dependent on the implementation of the components by annotations `@Order`, `@Priority` and Ordered interface.

```java
@Component
@Order(1)
public class SocketRecordsReader implements RecordsReader {
    @Override
    public Collection<Record> readRecords() {
        return null;
    }
}
```
```java
@Component
@Priority(2)
public class WebServiceRecordsReader implements RecordsReader {
    @Override
    public Collection<Record> readRecords() {
        return null;
    }
}
```
```java
@Component
public class FileRecordsReader implements RecordsReader, Ordered {
    @Override
    public Collection<Record> readRecords() {
        return null;
    }

    @Override
    public int getOrder() {
        return 3;
    }
}
```
```java
@Component
public class DbRecordsReader implements RecordsReader {
    @Override
    public Collection<Record> readRecords() {
        return Collections.emptyList();
    }
}
```
<br>
<br>

### 19. What is a proxy object and what are the two different types of proxy objects Spring can create? What are the limitations of these two types of proxies? What is the power of a proxy object and what are the disadvantages? 

A proxy object is an object that adds additional logic on top of the object that is being proxied without having to modify the code of the proxied object. The proxy object has the same public methods as the object that is being proxied, and it should be as much as possible indistinguishable from the proxies object. When a method is involked on the Proxy Object, additional code, usually before and after sections are involked, also code from the proxies object is invoked by the Proxy Object. 

The Spring Framework supports two kinds of proxies:

- JDK Dynamic Proxy - used by default if target object implements interface
- CGLIB Proxy - used when target does not implement any interface

Limitations of JDK Proxy:
- Requires proxy object to implement the interface
- Only interface methods will be proxied 
- No support for self-invocation

Limitations of CBLIB Proxy:
- Does not work for final classes or final methods 
- No support for self-invocation 

Proxy Advantages:

- Ability to change behaviour of existing beans without changing original code
- Separation of concerns (loggin, transactions, security...)

Proxy Disadvantages:

- May create code hard to debug
- Needs to use unchecked exception for exceptions not declared in original method
- May cause performance issues if before/after section in proxy code us using IO
- May cause unexpected equals operator results since Proxy Object and Proxied Object are two different objects

