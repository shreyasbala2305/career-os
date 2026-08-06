# Spring Boot Interview Handbook

This handbook contains high-frequency Spring Boot interview questions commonly asked in product companies, startups, and service-based organizations.

Each question includes:

- Interview-ready answer
- Additional explanation
- Common mistakes
- Repository reference (where applicable)

---

# Section 1 — Spring Framework & Spring Boot Basics

---

## Q1. What is Spring Framework?

### Answer

Spring Framework is an open-source Java framework used to build enterprise applications.

Its primary goal is to simplify Java development by providing infrastructure for:

- Inversion of Control (IoC)
- Dependency Injection (DI)
- Spring MVC
- Spring Data
- Transaction Management
- Security
- Aspect-Oriented Programming (AOP)

Instead of manually creating and managing objects, Spring manages them through its IoC Container.

### Additional Notes

Before Spring, enterprise applications were tightly coupled because developers had to create dependencies manually using the `new` keyword.

Spring introduced Dependency Injection, making applications easier to maintain, test, and extend.

### Common Mistake

❌ Spring is only used to build REST APIs.

✅ Spring is a complete enterprise framework that supports web applications, REST APIs, batch processing, messaging, security, cloud-native applications, and more.

### Repository Reference

- `docs/01-spring-overview.md`

---

## Q2. What is Spring Boot?

### Answer

Spring Boot is an extension of the Spring Framework that simplifies application development.

It provides:

- Auto Configuration
- Starter Dependencies
- Embedded Tomcat
- Production-ready features
- Opinionated defaults

Spring Boot allows developers to focus on business logic rather than infrastructure configuration.

### Additional Notes

Without Spring Boot, developers must manually configure many components such as:

- DispatcherServlet
- ViewResolver
- Jackson
- Embedded Server
- Bean definitions

Spring Boot performs these configurations automatically.

### Common Mistake

❌ Spring Boot replaces Spring Framework.

✅ Spring Boot is built on top of Spring Framework and uses Spring internally.

### Repository Reference

- `docs/01-spring-overview.md`

---

## Q3. Difference between Spring Framework and Spring Boot?

### Answer

| Spring Framework | Spring Boot |
|------------------|-------------|
| Core Framework | Built on Spring |
| Manual Configuration | Auto Configuration |
| External Server Required | Embedded Tomcat |
| More Boilerplate | Less Boilerplate |
| Flexible | Convention over Configuration |

### Interview Tip

If asked which one is used in industry, answer:

> Most modern Java backend applications use Spring Boot because it significantly reduces configuration while leveraging the full power of the Spring Framework.

### Common Mistake

❌ Spring Boot is a completely different framework.

✅ Spring Boot is an extension of the Spring Framework.

### Repository Reference

- `docs/01-spring-overview.md`

---

## Q4. Why was Spring Boot introduced?

### Answer

Spring Boot was introduced to reduce the complexity of building Spring applications.

It solves problems such as:

- Manual configuration
- XML configuration
- Dependency management
- External server setup
- Boilerplate code

This allows developers to build production-ready applications much faster.

### Additional Notes

Spring Boot follows the principle of **Convention over Configuration**, meaning sensible defaults are provided while still allowing customization when required.

### Common Mistake

❌ Spring Boot removes all configuration.

✅ Spring Boot provides sensible defaults, but developers can still customize the configuration whenever needed.

### Repository Reference

- `docs/01-spring-overview.md`

---

## Q5. What is Embedded Tomcat?

### Answer

Embedded Tomcat is a web server packaged inside a Spring Boot application.

Instead of deploying a WAR file to an external Tomcat server, Spring Boot packages Tomcat within the executable JAR.

Applications can be started using:

```bash
java -jar application.jar
```

### Additional Notes

Traditional deployment:

```
Application
      │
      ▼
WAR
      │
      ▼
External Tomcat
```

Spring Boot deployment:

```
Application
      │
      ▼
Embedded Tomcat
      │
      ▼
Executable JAR
```

### Common Mistake

❌ Spring Boot does not use Tomcat.

✅ Spring Boot uses Tomcat by default; it simply embeds it within the application.

### Repository Reference

- `docs/01-spring-overview.md`

---

## Q6. What are Starter Dependencies?

### Answer

Starter Dependencies are curated dependency bundles provided by Spring Boot.

For example:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

This single dependency includes:

- Spring MVC
- Embedded Tomcat
- Jackson
- Validation

### Additional Notes

Starter dependencies reduce dependency conflicts and ensure compatible library versions.

### Common Mistake

❌ Every Spring library must be added individually.

✅ Starter dependencies already bundle the required libraries.

### Repository Reference

- `docs/01-spring-overview.md`

---

## Q7. What is Auto Configuration?

### Answer

Auto Configuration is a Spring Boot feature that automatically configures infrastructure beans based on:

- Dependencies on the classpath
- Existing beans
- Application properties

For example, adding `spring-boot-starter-web` automatically configures:

- DispatcherServlet
- Jackson
- Embedded Tomcat
- Spring MVC

### Additional Notes

Developers can override any auto-configured bean if customization is required.

### Common Mistake

❌ Auto Configuration removes developer control.

✅ Auto Configuration provides defaults that can be customized or replaced.

### Repository Reference

- `docs/02-spring-architecture.md`

---

## Q8. What happens when `SpringApplication.run()` is executed?

### Answer

`SpringApplication.run()` bootstraps the entire Spring Boot application.

The startup sequence is:

```
main()
      │
      ▼
SpringApplication.run()
      │
      ▼
Create ApplicationContext
      │
      ▼
Component Scan
      │
      ▼
Bean Creation
      │
      ▼
Dependency Injection
      │
      ▼
@PostConstruct
      │
      ▼
Embedded Tomcat Starts
      │
      ▼
Application Ready
```

After these steps, the application is ready to accept HTTP requests.

### Additional Notes

During startup, Spring also:

- Loads configuration files
- Initializes logging
- Registers beans
- Applies auto configuration
- Starts the embedded server

### Common Mistake

❌ `SpringApplication.run()` only starts Tomcat.

✅ Starting Tomcat is only one step. It also creates the ApplicationContext, performs component scanning, creates beans, injects dependencies, and initializes the Spring application.

### Repository Reference

- `SpringbootRequestLifecycleApplication.java`
- `docs/02-spring-architecture.md`

# Section 2 — Spring Boot Architecture

---

## Q9. What is ApplicationContext?

### Answer

ApplicationContext is the central container of the Spring Framework.

It is responsible for:

- Creating Spring Beans
- Managing Bean lifecycle
- Performing Dependency Injection
- Loading configuration
- Publishing application events
- Managing application resources

Every Spring Boot application has exactly one primary `ApplicationContext`.

### Additional Notes

When the application starts,

```java
SpringApplication.run()
```

creates the `ApplicationContext`.

After that,

- Component Scanning begins
- Beans are created
- Dependencies are injected
- Embedded Tomcat starts

The application is then ready to receive HTTP requests.

### Common Mistake

❌ ApplicationContext is only used to store Beans.

✅ It manages the complete lifecycle of the application including Bean creation, dependency injection, events, resources and configuration.

### Repository Reference

- `docs/02-spring-architecture.md`

---

## Q10. What is BeanFactory?

### Answer

BeanFactory is the simplest implementation of the Spring IoC Container.

Its primary responsibilities are:

- Creating Beans
- Storing Beans
- Returning Beans when requested

It provides basic dependency management.

### Additional Notes

Most Spring Boot applications do **not** interact with BeanFactory directly.

Instead,

Spring Boot uses **ApplicationContext**, which extends BeanFactory and provides many additional enterprise features.

### Common Mistake

❌ BeanFactory is used directly in Spring Boot applications.

✅ Spring Boot primarily uses ApplicationContext.

### Repository Reference

- `docs/12-ioc-container.md`

---

## Q11. Difference between BeanFactory and ApplicationContext?

### Answer

| BeanFactory | ApplicationContext |
|-------------|--------------------|
| Basic IoC Container | Advanced IoC Container |
| Basic Bean management | Enterprise features |
| Lazy Bean initialization | Eager Singleton initialization |
| Limited functionality | Event publishing, i18n, resource loading |
| Rarely used directly | Used by Spring Boot |

### Additional Notes

ApplicationContext extends BeanFactory.

Everything BeanFactory can do,

ApplicationContext can also do,

plus many enterprise-level features.

### Common Mistake

❌ BeanFactory and ApplicationContext are completely different containers.

✅ ApplicationContext extends BeanFactory.

### Repository Reference

- `docs/12-ioc-container.md`

---

## Q12. What is Component Scanning?

### Answer

Component Scanning is the process through which Spring automatically discovers application components.

Spring scans the package containing the main application class and all its sub-packages.

Classes annotated with:

- `@Component`
- `@Service`
- `@Repository`
- `@Controller`
- `@RestController`
- `@Configuration`

are automatically registered as Spring Beans.

### Additional Notes

Without Component Scanning,

every Bean would need to be configured manually.

Component Scanning greatly reduces configuration effort.

### Common Mistake

❌ Spring scans the entire classpath.

✅ Spring scans only the package of the main application class and its sub-packages unless configured otherwise.

### Repository Reference

- `docs/02-spring-architecture.md`

---

## Q13. How does Spring create a Bean?

### Answer

Spring follows these steps:

```
Component Scan
        │
        ▼
Bean Definition Created
        │
        ▼
Object Instantiated
        │
        ▼
Dependencies Injected
        │
        ▼
@PostConstruct
        │
        ▼
Bean Ready
```

After initialization,

the Bean becomes available throughout the application.

### Additional Notes

Spring creates Singleton Beans during application startup by default.

Prototype Beans are created whenever requested.

### Common Mistake

❌ Beans are created only when they are first used.

✅ Singleton Beans are usually created during application startup.

### Repository Reference

- `docs/11-bean-lifecycle.md`

---

## Q14. What is Dependency Injection?

### Answer

Dependency Injection (DI) is the process of supplying required dependencies to an object instead of allowing the object to create them manually.

Without Dependency Injection

```java
StudentRepository repository =
        new StudentRepository();
```

With Dependency Injection

```java
@RequiredArgsConstructor
@Service
public class StudentService {

    private final StudentRepository repository;

}
```

Spring automatically injects the required dependency.

### Additional Notes

Dependency Injection promotes:

- Loose coupling
- Better testing
- Easier maintenance
- Better scalability

### Common Mistake

❌ Dependency Injection means using `@Autowired`.

✅ `@Autowired` is one way of performing Dependency Injection. Constructor Injection is now the recommended approach.

### Repository Reference

- `docs/12-ioc-container.md`

---

## Q15. What is Inversion of Control (IoC)?

### Answer

Inversion of Control (IoC) is a design principle where the responsibility for creating and managing objects is transferred from the application code to the Spring Container.

Instead of writing

```java
new StudentService();
```

Spring creates the object automatically.

### Additional Notes

IoC is the principle.

Dependency Injection is the mechanism Spring uses to implement IoC.

### Common Mistake

❌ IoC and Dependency Injection are the same.

✅ IoC is the concept.

Dependency Injection is one implementation of IoC.

### Repository Reference

- `docs/12-ioc-container.md`

---

## Q16. Explain the Spring Boot startup sequence.

### Answer

The startup sequence is:

```
main()
        │
        ▼
SpringApplication.run()
        │
        ▼
ApplicationContext Created
        │
        ▼
Component Scanning
        │
        ▼
Bean Definitions Registered
        │
        ▼
Beans Instantiated
        │
        ▼
Dependencies Injected
        │
        ▼
@PostConstruct Executed
        │
        ▼
Embedded Tomcat Starts
        │
        ▼
Application Ready
```

### Additional Notes

Only after all these steps are completed does Spring Boot begin accepting incoming HTTP requests.

### Common Mistake

❌ The application starts only after Tomcat starts.

✅ Tomcat starts near the end of the startup process. Before that, Spring creates the ApplicationContext, scans components, creates Beans and injects dependencies.

### Repository Reference

- `SpringbootRequestLifecycleApplication.java`
- `docs/02-spring-architecture.md`
- `docs/11-bean-lifecycle.md`

# Section 3 — DispatcherServlet & Spring MVC

---

## Q17. What is DispatcherServlet?

### Answer

DispatcherServlet is the **Front Controller** of Spring MVC.

Every incoming HTTP request first reaches the DispatcherServlet.

It coordinates the complete request lifecycle by:

- Receiving HTTP requests
- Finding the appropriate controller
- Executing the controller
- Processing the response
- Returning the HTTP response to the client

### Additional Notes

DispatcherServlet does not contain business logic.

Instead, it delegates work to components such as:

- HandlerMapping
- HandlerAdapter
- Interceptors
- HttpMessageConverters

### Common Mistake

❌ DispatcherServlet executes business logic.

✅ DispatcherServlet only coordinates request processing. Business logic belongs in the Service layer.

### Repository Reference

- `DispatcherDemoController.java`
- `docs/03-dispatcherservlet.md`

---

## Q18. Explain the Front Controller Pattern.

### Answer

The Front Controller Pattern is a design pattern where all incoming requests pass through a single controller before being routed to the appropriate handler.

In Spring MVC,

```
DispatcherServlet
```

acts as the Front Controller.

### Additional Notes

Without Front Controller

```
Client

↓

Controller A

Client

↓

Controller B

Client

↓

Controller C
```

With Front Controller

```
Client

↓

DispatcherServlet

↓

Controller A

↓

Controller B

↓

Controller C
```

This centralizes request processing.

### Common Mistake

❌ Every controller receives requests directly.

✅ DispatcherServlet receives every request first.

### Repository Reference

- `docs/03-dispatcherservlet.md`

---

## Q19. What is HandlerMapping?

### Answer

HandlerMapping identifies which controller method should process an incoming request.

Example

```
GET /students/1
```

HandlerMapping searches all mapped endpoints and returns:

```
StudentController

↓

getStudentById()
```

### Additional Notes

HandlerMapping only identifies the correct handler.

It does **not** execute the controller.

### Common Mistake

❌ HandlerMapping executes controller methods.

✅ It only locates the appropriate controller.

### Repository Reference

- `docs/03-dispatcherservlet.md`

---

## Q20. What is HandlerAdapter?

### Answer

HandlerAdapter is responsible for invoking the controller method selected by HandlerMapping.

It also resolves method parameters such as:

- `@PathVariable`
- `@RequestBody`
- `@RequestParam`
- `@RequestHeader`

### Additional Notes

For example,

```java
@GetMapping("/{id}")
public StudentDTO getStudent(
        @PathVariable Long id)
```

HandlerAdapter extracts the value from the URL and passes it to the method.

### Common Mistake

❌ DispatcherServlet directly invokes controller methods.

✅ DispatcherServlet delegates execution to HandlerAdapter.

### Repository Reference

- `docs/03-dispatcherservlet.md`

---

## Q21. What is HttpMessageConverter?

### Answer

HttpMessageConverter converts Java objects to HTTP responses and HTTP request bodies into Java objects.

For REST APIs,

Spring Boot commonly uses:

```
MappingJackson2HttpMessageConverter
```

which internally uses Jackson.

### Additional Notes

Request

```
JSON

↓

StudentDTO
```

Response

```
StudentDTO

↓

JSON
```

The conversion is automatic.

### Common Mistake

❌ Controllers convert JSON manually.

✅ Spring automatically converts JSON using HttpMessageConverter.

### Repository Reference

- `docs/03-dispatcherservlet.md`
- `docs/10-dto-pattern.md`

---

## Q22. How does `@RequestBody` work internally?

### Answer

When a request contains JSON,

Spring follows these steps:

```
HTTP Request

↓

DispatcherServlet

↓

HandlerAdapter

↓

HttpMessageConverter

↓

Jackson

↓

Java Object

↓

Controller
```

The controller receives a fully populated Java object.

### Additional Notes

Example

```java
@PostMapping
public StudentDTO createStudent(
        @RequestBody StudentDTO dto)
```

No manual JSON parsing is required.

### Common Mistake

❌ `@RequestBody` parses JSON itself.

✅ Jackson performs the JSON conversion through HttpMessageConverter.

### Repository Reference

- `StudentController.java`
- `docs/10-dto-pattern.md`

---

## Q23. What happens internally when a request reaches Spring Boot?

### Answer

The complete request flow is:

```
HTTP Client

↓

Embedded Tomcat

↓

Servlet Filter

↓

DispatcherServlet

↓

HandlerMapping

↓

HandlerAdapter

↓

Interceptor (preHandle)

↓

Controller

↓

Service

↓

Repository

↓

Database
```

The response follows the reverse path.

### Additional Notes

This is one of the most common backend interview questions.

Be able to explain each component's responsibility.

### Common Mistake

❌ Tomcat directly calls the Controller.

✅ Tomcat forwards requests to DispatcherServlet.

### Repository Reference

- `docs/04-request-lifecycle.md`

---

## Q24. How does Spring know which controller should handle a request?

### Answer

Spring uses the combination of:

- Request URL
- HTTP Method
- Mapping annotations

Example

```java
@GetMapping("/{id}")
```

When the request

```
GET /students/1
```

arrives,

HandlerMapping matches it with the above controller method.

### Additional Notes

Spring builds these mappings during application startup.

This makes request resolution very fast.

### Common Mistake

❌ Spring scans all controllers for every request.

✅ The mappings are prepared during startup and efficiently looked up at runtime.

### Repository Reference

- `StudentController.java`
- `docs/03-dispatcherservlet.md`

---

## Q25. What is the difference between `@Controller` and `@RestController`?

### Answer

| @Controller | @RestController |
|--------------|-----------------|
| Used for MVC applications | Used for REST APIs |
| Returns Views | Returns JSON/XML |
| Requires `@ResponseBody` for REST responses | Includes `@ResponseBody` automatically |

### Additional Notes

`@RestController` is equivalent to:

```java
@Controller
@ResponseBody
```

This is why REST APIs generally use `@RestController`.

### Common Mistake

❌ `@Controller` cannot return JSON.

✅ It can return JSON when combined with `@ResponseBody`.

### Repository Reference

- `StudentController.java`
- `docs/07-controller-layer.md`

---

## Q26. Explain the complete Spring MVC request flow.

### Answer

```
HTTP Request

↓

Embedded Tomcat

↓

Filter

↓

DispatcherServlet

↓

HandlerMapping

↓

HandlerAdapter

↓

Interceptor (preHandle)

↓

Controller

↓

Service

↓

Repository

↓

Database

↓

Repository

↓

Service

↓

Controller

↓

Interceptor (postHandle)

↓

HttpMessageConverter (Jackson)

↓

Interceptor (afterCompletion)

↓

Filter

↓

HTTP Response
```

### Additional Notes

This is one of the highest-frequency Spring Boot interview questions.

Interviewers often ask candidates to explain every component in this flow.

### Common Mistake

❌ Memorizing only the flow.

✅ Understand the responsibility of each component and why it exists.

### Repository Reference

- `docs/04-request-lifecycle.md`
- `springboot-request-lifecycle-demo`

# Section 4 — Complete Request Lifecycle

---

## Q27. Explain the complete request lifecycle in a Spring Boot application.

### Answer

When a client sends an HTTP request, it passes through multiple Spring and Servlet components before a response is returned.

The complete flow is:

```
Client

↓

Embedded Tomcat

↓

Servlet Filter

↓

DispatcherServlet

↓

HandlerMapping

↓

HandlerAdapter

↓

Interceptor (preHandle)

↓

Controller

↓

Service

↓

Repository

↓

Hibernate

↓

Database

↓

Hibernate

↓

Repository

↓

Service

↓

Controller

↓

Interceptor (postHandle)

↓

HttpMessageConverter (Jackson)

↓

Interceptor (afterCompletion)

↓

Servlet Filter

↓

Embedded Tomcat

↓

HTTP Response
```

### Additional Notes

Understanding this flow is one of the most important Spring Boot interview topics because it explains how different framework components work together.

### Common Mistake

❌ Saying the request directly reaches the Controller.

✅ Every request first reaches Embedded Tomcat and DispatcherServlet before the Controller.

### Repository Reference

- `docs/04-request-lifecycle.md`

---

## Q28. What happens after a client sends an HTTP request?

### Answer

After a client sends an HTTP request:

1. Embedded Tomcat accepts the request.
2. Servlet Filters execute.
3. DispatcherServlet receives the request.
4. HandlerMapping finds the correct controller.
5. HandlerAdapter invokes the controller method.
6. Business logic executes.
7. The response is converted into JSON.
8. The HTTP response is returned to the client.

### Additional Notes

Tomcat is responsible for handling HTTP communication.

Spring MVC starts at DispatcherServlet.

### Common Mistake

❌ Tomcat executes controller methods.

✅ DispatcherServlet coordinates request processing.

### Repository Reference

- `docs/04-request-lifecycle.md`

---

## Q29. Why does every request go through DispatcherServlet?

### Answer

DispatcherServlet acts as the Front Controller.

Having a single entry point allows Spring to:

- Centralize routing
- Apply Filters and Interceptors consistently
- Handle exceptions globally
- Convert requests and responses
- Maintain a consistent request lifecycle

### Additional Notes

Without DispatcherServlet, each controller would have to implement request processing independently.

### Common Mistake

❌ DispatcherServlet exists only for routing.

✅ It coordinates the entire Spring MVC workflow.

### Repository Reference

- `docs/03-dispatcherservlet.md`

---

## Q30. What happens after the Controller returns an object?

### Answer

The Controller returns a Java object such as a DTO.

Spring then:

```
StudentDTO

↓

HttpMessageConverter

↓

Jackson

↓

JSON

↓

HTTP Response
```

The client never receives the Java object directly.

### Additional Notes

For REST APIs,

Spring Boot automatically selects

```
MappingJackson2HttpMessageConverter
```

to convert Java objects into JSON.

### Common Mistake

❌ Controller returns JSON.

✅ Controller returns a Java object. Jackson converts it into JSON.

### Repository Reference

- `docs/10-dto-pattern.md`

---

## Q31. What is Jackson?

### Answer

Jackson is a Java library used by Spring Boot to serialize Java objects into JSON and deserialize JSON into Java objects.

Example

```
JSON

↓

StudentDTO

↓

Controller
```

and

```
StudentDTO

↓

JSON

↓

HTTP Response
```

### Additional Notes

Spring Boot automatically configures Jackson when using

```
spring-boot-starter-web
```

### Common Mistake

❌ Controllers manually convert JSON.

✅ Jackson performs conversion automatically.

### Repository Reference

- `docs/10-dto-pattern.md`

---

## Q32. What is the execution order of Filters, Interceptors and Controllers?

### Answer

```
HTTP Request

↓

Servlet Filter

↓

DispatcherServlet

↓

Interceptor (preHandle)

↓

Controller

↓

Interceptor (postHandle)

↓

Jackson

↓

Interceptor (afterCompletion)

↓

Servlet Filter

↓

HTTP Response
```

### Additional Notes

Remember this sequence because it is commonly asked during interviews.

### Common Mistake

❌ Interceptors execute before Filters.

✅ Filters execute before Spring MVC begins.

### Repository Reference

- `docs/05-filter.md`
- `docs/06-interceptor.md`

---

## Q33. At which stage does validation occur?

### Answer

Validation occurs before the controller method executes.

Flow

```
HTTP Request

↓

DispatcherServlet

↓

HandlerAdapter

↓

Validation

↓

Controller
```

If validation fails,

Spring throws

```
MethodArgumentNotValidException
```

which is typically handled by a global exception handler.

### Additional Notes

Validation is triggered using

```java
@Valid
@RequestBody StudentDTO dto
```

### Common Mistake

❌ Validation happens inside the Service.

✅ Bean Validation occurs before the controller method is invoked.

### Repository Reference

- `StudentController.java`
- `docs/13-exception-handling.md`

---

## Q34. What happens if an exception occurs during request processing?

### Answer

If an exception occurs,

Spring stops normal execution and forwards the exception to the Global Exception Handler.

Flow

```
Controller

↓

Service

↓

Exception

↓

@RestControllerAdvice

↓

ErrorResponse

↓

JSON

↓

Client
```

### Additional Notes

This keeps controllers clean and ensures consistent API error responses.

### Common Mistake

❌ Every controller should contain try-catch blocks.

✅ Centralize exception handling using `@RestControllerAdvice`.

### Repository Reference

- `GlobalExceptionHandler.java`
- `docs/13-exception-handling.md`

---

## Q35. How would you explain the complete request lifecycle in an interview?

### Answer

A concise interview answer:

> A client sends an HTTP request to the embedded Tomcat server. The request first passes through Servlet Filters and then reaches DispatcherServlet, which acts as the Front Controller. DispatcherServlet uses HandlerMapping to locate the appropriate controller and HandlerAdapter to invoke it. The controller delegates business logic to the Service layer, which interacts with the Repository layer. The Repository communicates with the database through Hibernate. The response then travels back through the Service and Controller. Spring uses HttpMessageConverter with Jackson to serialize the response object into JSON before sending it back to the client.

### Interview Tip

Practice explaining this flow without looking at notes.

Being able to explain it confidently is a strong indicator of Spring Boot fundamentals.

### Repository Reference

- `springboot-request-lifecycle-demo`
- `docs/04-request-lifecycle.md`

---

## Q36. Which parts of the request lifecycle did you implement in this repository?

### Answer

This repository demonstrates:

- Embedded Tomcat
- DispatcherServlet
- Request Mapping
- Filters
- Interceptors
- Controllers
- Services
- Repositories
- H2 Database
- DTO Pattern
- Jackson Serialization
- Bean Lifecycle
- IoC Container
- Global Exception Handling

### Additional Notes

In an interview, you can confidently say:

> "I built a Spring Boot project that demonstrates the complete request lifecycle from the HTTP request entering Embedded Tomcat until the JSON response is returned to the client."

This is much stronger than simply saying you've built CRUD APIs.

### Repository Reference

- Entire Repository

# Section 5 — Filters & Interceptors

---

## Q37. What is a Servlet Filter?

### Answer

A Servlet Filter is a component provided by the Java Servlet API that intercepts every HTTP request before it enters Spring MVC and every HTTP response before it leaves the application.

Filters execute at the Servlet Container level (Embedded Tomcat), making them independent of Spring MVC.

### Additional Notes

Typical use cases include:

- Request logging
- Authentication
- CORS
- Compression
- Character Encoding
- Request timing

Execution Flow

```
HTTP Request

↓

Embedded Tomcat

↓

Servlet Filter

↓

DispatcherServlet
```

### Common Mistake

❌ Filters are part of Spring MVC.

✅ Filters belong to the Servlet API and execute before Spring MVC.

### Repository Reference

- `LoggingFilter.java`
- `docs/05-filter.md`

---

## Q38. What is an Interceptor?

### Answer

An Interceptor is a Spring MVC component that intercepts requests after they enter Spring MVC but before the controller executes.

Unlike Filters, Interceptors are aware of Spring MVC concepts such as controllers and handler methods.

### Additional Notes

Typical use cases include:

- Authentication
- Authorization
- Logging
- Performance Monitoring
- Audit Logging

Execution Flow

```
DispatcherServlet

↓

Interceptor

↓

Controller
```

### Common Mistake

❌ Interceptors execute before Filters.

✅ Filters always execute first.

### Repository Reference

- `RequestInterceptor.java`
- `docs/06-interceptor.md`

---

## Q39. Difference between Filter and Interceptor?

### Answer

| Filter | Interceptor |
|----------|-------------|
| Servlet API | Spring MVC |
| Executes before DispatcherServlet | Executes after DispatcherServlet |
| Not aware of Controllers | Knows the Controller and Handler |
| Configured by Servlet Container | Configured by Spring MVC |
| Suitable for CORS, Encoding, Compression | Suitable for Authentication, Authorization, Logging |

### Additional Notes

Think of it like this:

```
Tomcat

↓

Filter

↓

Spring MVC

↓

Interceptor

↓

Controller
```

### Interview Tip

This is one of the most frequently asked Spring Boot interview questions.

Don't simply memorize the table—explain **where each executes**.

### Repository Reference

- `docs/05-filter.md`
- `docs/06-interceptor.md`

---

## Q40. What is the purpose of `preHandle()`?

### Answer

`preHandle()` executes before the controller method.

It decides whether the request should continue.

If it returns

```java
true
```

the request proceeds.

If it returns

```java
false
```

Spring stops processing the request.

### Additional Notes

Typical uses:

- Authentication
- Authorization
- Request validation
- Logging

### Common Mistake

❌ `preHandle()` executes after the controller.

✅ It executes before the controller.

### Repository Reference

- `RequestInterceptor.java`

---

## Q41. What is the purpose of `postHandle()`?

### Answer

`postHandle()` executes after the controller method finishes but before the response is written to the client.

Typical uses include:

- Logging
- Modifying response models
- Performance monitoring
- Adding headers

### Additional Notes

At this stage,

the controller has completed execution,

but the response has not yet been sent.

### Common Mistake

❌ `postHandle()` executes after the response is sent.

✅ It executes before the response is committed.

### Repository Reference

- `RequestInterceptor.java`

---

## Q42. What is the purpose of `afterCompletion()`?

### Answer

`afterCompletion()` executes after the complete request lifecycle has finished and the response has been sent.

Typical uses include:

- Resource cleanup
- Final logging
- Performance calculation
- Releasing resources

### Additional Notes

Execution Order

```
preHandle()

↓

Controller

↓

postHandle()

↓

Jackson

↓

afterCompletion()
```

### Common Mistake

❌ `afterCompletion()` executes before Jackson.

✅ It executes after the complete request processing.

### Repository Reference

- `RequestInterceptor.java`

---

## Q43. Can an Interceptor stop a request?

### Answer

Yes.

Returning

```java
false
```

from `preHandle()` prevents the request from reaching the controller.

Example

```java
@Override
public boolean preHandle(...) {

    if (!authenticated) {
        return false;
    }

    return true;

}
```

### Additional Notes

This is commonly used for authentication and authorization.

### Common Mistake

❌ Interceptors can only observe requests.

✅ They can stop request processing entirely.

### Repository Reference

- `docs/06-interceptor.md`

---

## Q44. Can multiple Filters and Interceptors exist?

### Answer

Yes.

Both Filters and Interceptors can be chained together.

Example

```
Request

↓

Filter A

↓

Filter B

↓

DispatcherServlet

↓

Interceptor A

↓

Interceptor B

↓

Controller
```

Each component executes in the configured order.

### Additional Notes

Spring allows ordering Filters and Interceptors to control execution sequence.

### Common Mistake

❌ Only one Filter or one Interceptor can exist.

✅ Multiple Filters and Interceptors are commonly used in production systems.

### Repository Reference

- `LoggingFilter.java`
- `RequestInterceptor.java`

---

## Q45. When should you use a Filter instead of an Interceptor?

### Answer

Use a Filter when the functionality should execute before Spring MVC begins.

Examples:

- CORS
- Compression
- Character Encoding
- Request Logging
- Security Headers

### Additional Notes

Filters are framework-independent because they belong to the Servlet API.

### Interview Tip

If the task does **not require controller information**, a Filter is usually the better choice.

### Repository Reference

- `docs/05-filter.md`

---

## Q46. When should you use an Interceptor instead of a Filter?

### Answer

Use an Interceptor when the functionality depends on Spring MVC.

Examples:

- Authentication
- Authorization
- Measuring controller execution time
- Audit logging
- Accessing HandlerMethod information

### Additional Notes

Interceptors know:

- Which controller executes
- Which method executes
- Which annotations are present

Filters do not.

### Common Mistake

❌ Filters and Interceptors are interchangeable.

✅ They solve different problems and often work together in the same application.

### Repository Reference

- `docs/06-interceptor.md`

# Section 6 — IoC & Dependency Injection

---

## Q47. What is Inversion of Control (IoC)?

### Answer

Inversion of Control (IoC) is a design principle in which the responsibility of creating, configuring and managing objects is transferred from the application code to the Spring Container.

Instead of developers creating objects manually, Spring creates and manages them.

Without IoC

```java
StudentRepository repository = new StudentRepository();
StudentService service = new StudentService(repository);
```

With IoC

```java
@Service
@RequiredArgsConstructor
public class StudentService {

    private final StudentRepository repository;

}
```

Spring automatically creates and injects the repository.

### Additional Notes

Think of IoC as:

```
Developer

↓

Defines Classes

↓

Spring Container

↓

Creates Objects

↓

Injects Dependencies
```

### Common Mistake

❌ IoC means using `@Autowired`.

✅ IoC is a design principle. Dependency Injection is one way Spring implements IoC.

### Repository Reference

- `docs/12-ioc-container.md`

---

## Q48. What is Dependency Injection (DI)?

### Answer

Dependency Injection is the process of providing an object's dependencies instead of allowing the object to create them manually.

Spring performs Dependency Injection automatically using the IoC Container.

Example

```java
@RequiredArgsConstructor
@Service
public class StudentService {

    private final StudentRepository repository;

}
```

### Additional Notes

Benefits:

- Loose coupling
- Better testing
- Easier maintenance
- Easier mocking
- Better scalability

### Common Mistake

❌ Dependency Injection only works using `@Autowired`.

✅ Constructor Injection, Setter Injection and Field Injection are all forms of Dependency Injection.

### Repository Reference

- `docs/12-ioc-container.md`

---

## Q49. Difference between IoC and Dependency Injection?

### Answer

| IoC | Dependency Injection |
|-----|----------------------|
| Design Principle | Implementation Technique |
| Transfers control to Spring | Supplies dependencies |
| Broad concept | Specific mechanism |
| Achieved using DI | One way to implement IoC |

### Additional Notes

Interview Tip

A simple answer is:

> IoC answers **who creates objects**, while Dependency Injection answers **how those objects receive their dependencies**.

### Common Mistake

❌ IoC and DI are exactly the same.

✅ Dependency Injection is Spring's implementation of the IoC principle.

### Repository Reference

- `docs/12-ioc-container.md`

---

## Q50. Why is Constructor Injection recommended?

### Answer

Constructor Injection is the recommended approach because it:

- Makes dependencies mandatory
- Supports immutable fields (`final`)
- Improves testability
- Makes dependencies explicit
- Prevents partially initialized objects

Example

```java
@RequiredArgsConstructor
@Service
public class StudentService {

    private final StudentRepository repository;

}
```

### Additional Notes

Spring Boot automatically injects dependencies through the generated constructor.

### Common Mistake

❌ Constructor Injection requires `@Autowired`.

✅ Since Spring Framework 4.3, a single constructor is automatically used for injection.

### Repository Reference

- `StudentService.java`
- `docs/12-ioc-container.md`

---

## Q51. Why is Field Injection discouraged?

### Answer

Example

```java
@Autowired
private StudentRepository repository;
```

Although it works, Field Injection is generally discouraged because:

- Dependencies are hidden
- Difficult to unit test
- Cannot use final fields
- Encourages mutable objects

### Additional Notes

Most production Spring Boot applications prefer Constructor Injection.

### Common Mistake

❌ Field Injection is the recommended approach.

✅ Constructor Injection is recommended by Spring.

### Repository Reference

- `docs/12-ioc-container.md`

---

## Q52. What is Component Scanning?

### Answer

Component Scanning is the process by which Spring automatically discovers classes annotated with:

- `@Component`
- `@Service`
- `@Repository`
- `@Controller`
- `@RestController`
- `@Configuration`

These classes are automatically registered as Spring Beans.

### Additional Notes

Scanning starts from the package containing the main application class and continues through its sub-packages.

### Common Mistake

❌ Spring scans the entire project automatically.

✅ Spring scans only the configured base packages.

### Repository Reference

- `docs/02-spring-architecture.md`

---

## Q53. How does Spring create and inject Beans?

### Answer

Spring follows this sequence:

```
Application Starts

↓

Component Scan

↓

Bean Definition

↓

Object Creation

↓

Dependency Injection

↓

@PostConstruct

↓

Bean Ready
```

After initialization,

the Bean is stored inside the ApplicationContext.

### Additional Notes

Most Singleton Beans are created during application startup.

### Common Mistake

❌ Beans are created every time they are used.

✅ Singleton Beans are usually created once during startup.

### Repository Reference

- `docs/11-bean-lifecycle.md`

---

## Q54. What is a Circular Dependency?

### Answer

A Circular Dependency occurs when two or more Beans depend on each other.

Example

```
StudentService

↓

NotificationService

↓

StudentService
```

Neither Bean can be fully created because each requires the other.

### Additional Notes

Constructor Injection helps detect Circular Dependencies early.

The preferred solution is to redesign the classes to remove the circular dependency.

### Common Mistake

❌ Using `@Lazy` is the best solution.

✅ `@Lazy` can break the cycle, but redesigning the application is usually the better long-term solution.

### Repository Reference

- `docs/12-ioc-container.md`

---

## Q55. What is the ApplicationContext?

### Answer

ApplicationContext is the primary IoC Container in Spring Boot.

It manages:

- Bean creation
- Bean lifecycle
- Dependency Injection
- Events
- Resources
- Configuration

It acts as the central container for the application.

### Additional Notes

Every Spring Boot application has one primary ApplicationContext.

### Common Mistake

❌ ApplicationContext only stores Beans.

✅ It manages the complete lifecycle of the application.

### Repository Reference

- `docs/02-spring-architecture.md`

---

## Q56. Explain the Bean creation process.

### Answer

The Bean creation process is:

```
Component Scan

↓

Bean Definition

↓

Constructor Called

↓

Dependency Injection

↓

@PostConstruct

↓

Bean Ready

↓

Business Logic

↓

@PreDestroy

↓

Bean Destroyed
```

### Additional Notes

Understanding this sequence is important because it explains how Spring initializes application components before handling requests.

### Common Mistake

❌ `@PostConstruct` executes before Dependency Injection.

✅ Dependencies are injected first, then `@PostConstruct` is executed.

### Repository Reference

- `BeanLifecycleService.java`
- `docs/11-bean-lifecycle.md`

# Section 7 — Spring Bean Lifecycle

---

## Q57. What is a Spring Bean?

### Answer

A Spring Bean is an object that is created, configured, managed, and destroyed by the Spring IoC Container.

Instead of creating objects manually using the `new` keyword, Spring creates and manages them throughout their lifecycle.

Examples of Spring Beans include:

- Controllers
- Services
- Repositories
- Components
- Configuration classes

Example

```java
@Service
public class StudentService {

}
```

Spring automatically creates an instance of this class during application startup.

### Additional Notes

A normal Java object becomes a Spring Bean only when it is managed by the Spring Container.

### Common Mistake

❌ Every Java object is a Spring Bean.

✅ Only objects managed by the Spring Container are Spring Beans.

### Repository Reference

- `docs/11-bean-lifecycle.md`

---

## Q58. Explain the Spring Bean Lifecycle.

### Answer

The lifecycle of a Spring Bean consists of several phases managed by the Spring Container.

```
Application Starts

↓

Component Scan

↓

Bean Created

↓

Dependency Injection

↓

@PostConstruct

↓

Bean Ready

↓

Business Logic

↓

@PreDestroy

↓

Bean Destroyed
```

### Additional Notes

Each phase has a specific responsibility:

- Bean Creation
- Dependency Injection
- Initialization
- Usage
- Destruction

Spring manages the complete lifecycle automatically.

### Common Mistake

❌ Spring only creates Beans.

✅ Spring manages the complete lifecycle from creation to destruction.

### Repository Reference

- `docs/11-bean-lifecycle.md`

---

## Q59. What is `@PostConstruct`?

### Answer

`@PostConstruct` is a lifecycle annotation that executes immediately after dependency injection has completed.

It is commonly used for:

- Loading configuration
- Initializing caches
- Creating resources
- Startup logic

Example

```java
@PostConstruct
public void initialize() {

    System.out.println("Bean Initialized");

}
```

### Additional Notes

This method executes only once during the Bean lifecycle.

### Common Mistake

❌ `@PostConstruct` executes before Dependency Injection.

✅ Dependencies are injected first, then `@PostConstruct` executes.

### Repository Reference

- `BeanLifecycleService.java`
- `docs/11-bean-lifecycle.md`

---

## Q60. What is `@PreDestroy`?

### Answer

`@PreDestroy` is a lifecycle annotation that executes just before the Bean is destroyed.

Typical uses include:

- Closing files
- Closing database connections
- Releasing resources
- Cleanup operations

Example

```java
@PreDestroy
public void cleanup() {

    System.out.println("Cleaning Resources");

}
```

### Additional Notes

This method is typically called when the application shuts down.

### Common Mistake

❌ `@PreDestroy` executes after every request.

✅ It executes when the Bean is destroyed, usually during application shutdown.

### Repository Reference

- `BeanLifecycleService.java`
- `docs/11-bean-lifecycle.md`

---

## Q61. What is the default Bean scope in Spring?

### Answer

The default Bean scope in Spring is **Singleton**.

This means Spring creates only one instance of the Bean for the entire application.

Example

```
Application

↓

One StudentService Bean

↓

Used by Every Request
```

### Additional Notes

Singleton Beans are created during application startup unless lazy initialization is enabled.

### Common Mistake

❌ A new Bean is created for every HTTP request.

✅ Singleton Beans are shared across all requests.

### Repository Reference

- `docs/11-bean-lifecycle.md`

---

## Q62. What is Prototype scope?

### Answer

Prototype scope tells Spring to create a new Bean instance every time it is requested.

Example

```java
@Component
@Scope("prototype")
public class PrototypeBean {

}
```

Execution

```
Request 1

↓

New Bean

----------------

Request 2

↓

New Bean

----------------

Request 3

↓

New Bean
```

### Additional Notes

Unlike Singleton Beans, Prototype Beans are not shared.

### Common Mistake

❌ Prototype Beans behave like Singleton Beans.

✅ Every request for a Prototype Bean creates a new instance.

### Repository Reference

- `docs/11-bean-lifecycle.md`

---

## Q63. Difference between Singleton and Prototype scope?

### Answer

| Singleton | Prototype |
|------------|-----------|
| Default scope | Explicit scope |
| One Bean instance | New Bean instance every request |
| Shared across application | Independent instances |
| Memory efficient | More object creation |

### Additional Notes

Most Service, Repository and Controller Beans are Singleton.

Prototype scope is used only when a fresh object is required each time.

### Common Mistake

❌ Prototype is better because it creates fresh objects.

✅ Singleton is preferred unless there is a specific requirement for multiple instances.

### Repository Reference

- `docs/11-bean-lifecycle.md`

---

## Q64. What are the different Bean scopes available in Spring?

### Answer

Spring provides several Bean scopes:

| Scope | Description |
|--------|-------------|
| Singleton | One Bean per ApplicationContext |
| Prototype | New Bean every request |
| Request | One Bean per HTTP request |
| Session | One Bean per HTTP session |
| Application | One Bean per ServletContext |
| WebSocket | One Bean per WebSocket session |

### Additional Notes

For backend REST APIs,

Singleton is by far the most commonly used scope.

### Common Mistake

❌ Every Bean uses Prototype scope.

✅ Singleton is the default and most frequently used scope.

### Repository Reference

- `docs/11-bean-lifecycle.md`

---

## Q65. When does Spring destroy a Bean?

### Answer

Spring destroys Singleton Beans when the ApplicationContext is closed, usually during application shutdown.

During destruction,

Spring executes:

```
@PreDestroy

↓

Bean Removed

↓

Application Stops
```

### Additional Notes

Spring fully manages the lifecycle of Singleton Beans.

For Prototype Beans, Spring creates them but does not automatically manage their destruction.

### Common Mistake

❌ Spring automatically destroys every Prototype Bean.

✅ Spring manages Prototype Bean creation, but not their complete lifecycle after creation.

### Repository Reference

- `BeanLifecycleService.java`
- `docs/11-bean-lifecycle.md`

---

## Q66. How would you explain the Bean Lifecycle in an interview?

### Answer

A concise interview answer:

> During application startup, Spring performs component scanning and identifies classes annotated as Beans. It creates Bean instances, injects their dependencies, and executes any methods annotated with `@PostConstruct`. The Beans are then available for use throughout the application's lifetime. When the application shuts down, Spring invokes methods annotated with `@PreDestroy` before destroying the Beans.

### Interview Tip

Interviewers often ask this immediately after questions on IoC and Dependency Injection.

Explain the lifecycle in order rather than simply naming the annotations.

### Repository Reference

- `BeanLifecycleService.java`
- `docs/11-bean-lifecycle.md`

# Section 8 — REST Controllers & Request Mapping

---

## Q67. What is a REST Controller?

### Answer

A REST Controller is a Spring component responsible for handling HTTP requests and returning data directly as the HTTP response body.

It is created using the `@RestController` annotation.

Example

```java
@RestController
@RequestMapping("/students")
public class StudentController {

}
```

Unlike a traditional MVC Controller, a REST Controller does not return views. It typically returns JSON or XML.

### Additional Notes

`@RestController` is equivalent to:

```java
@Controller
@ResponseBody
```

This is why it is commonly used for REST APIs.

### Common Mistake

❌ `@RestController` returns HTML pages.

✅ It returns the response body directly, usually in JSON format.

### Repository Reference

- `StudentController.java`
- `docs/07-controller-layer.md`

---

## Q68. What is `@RequestMapping`?

### Answer

`@RequestMapping` maps HTTP requests to a controller or a specific controller method.

When placed on a class, it defines the base URL.

Example

```java
@RestController
@RequestMapping("/students")
public class StudentController {

}
```

All endpoints inside this controller will begin with:

```
/students
```

### Additional Notes

`@RequestMapping` can also specify:

- HTTP method
- URL path
- Headers
- Content type

However, specialized annotations like `@GetMapping` and `@PostMapping` are preferred for readability.

### Common Mistake

❌ `@RequestMapping` only maps URLs.

✅ It can also map HTTP methods, headers, consumes and produces conditions.

### Repository Reference

- `StudentController.java`
- `docs/07-controller-layer.md`

---

## Q69. Difference between `@GetMapping`, `@PostMapping`, `@PutMapping`, and `@DeleteMapping`?

### Answer

These annotations map controller methods to specific HTTP methods.

| Annotation | HTTP Method | Purpose |
|------------|-------------|---------|
| `@GetMapping` | GET | Retrieve data |
| `@PostMapping` | POST | Create data |
| `@PutMapping` | PUT | Update data |
| `@DeleteMapping` | DELETE | Delete data |
| `@PatchMapping` | PATCH | Partial update |

Example

```java
@GetMapping("/{id}")
public StudentDTO getStudent(@PathVariable Long id) {

}
```

### Additional Notes

Choosing the correct HTTP method improves API readability and follows REST principles.

### Common Mistake

❌ Using `POST` for every operation.

✅ Use the HTTP method that matches the operation being performed.

### Repository Reference

- `StudentController.java`

---

## Q70. What is `@PathVariable`?

### Answer

`@PathVariable` extracts values from the URL path and binds them to method parameters.

Example

```
GET /students/10
```

Controller

```java
@GetMapping("/{id}")
public StudentDTO getStudent(
        @PathVariable Long id) {

}
```

Spring automatically binds:

```
id = 10
```

### Additional Notes

Use `@PathVariable` when the value is part of the resource's identity.

### Common Mistake

❌ Using `@RequestParam` for URL path segments.

✅ Use `@PathVariable` for values embedded in the URL.

### Repository Reference

- `StudentController.java`
- `docs/07-controller-layer.md`

---

## Q71. What is `@RequestParam`?

### Answer

`@RequestParam` extracts query parameters from the request URL.

Example

```
GET /students?page=1&size=10
```

Controller

```java
@GetMapping
public List<StudentDTO> getStudents(
        @RequestParam int page,
        @RequestParam int size) {

}
```

Spring binds the query parameters automatically.

### Additional Notes

Use `@RequestParam` for filtering, sorting and pagination.

### Common Mistake

❌ Using `@PathVariable` for query parameters.

✅ Use `@RequestParam` for values after the `?` in the URL.

### Repository Reference

- `docs/07-controller-layer.md`

---

## Q72. What is `@RequestBody`?

### Answer

`@RequestBody` tells Spring to convert the HTTP request body into a Java object.

Example Request

```json
{
    "name":"John",
    "email":"john@example.com"
}
```

Controller

```java
@PostMapping
public StudentDTO createStudent(
        @RequestBody StudentDTO dto) {

}
```

Spring uses Jackson to perform the conversion automatically.

### Additional Notes

Internally,

```
JSON

↓

HttpMessageConverter

↓

Jackson

↓

StudentDTO
```

### Common Mistake

❌ `@RequestBody` parses JSON itself.

✅ Jackson performs the conversion through `HttpMessageConverter`.

### Repository Reference

- `StudentController.java`
- `docs/10-dto-pattern.md`

---

## Q73. What is `ResponseEntity`?

### Answer

`ResponseEntity` represents the complete HTTP response.

It allows developers to control:

- HTTP Status
- Response Headers
- Response Body

Example

```java
return ResponseEntity
        .status(HttpStatus.CREATED)
        .body(studentDTO);
```

### Additional Notes

`ResponseEntity` is useful when different status codes need to be returned.

Examples:

- 200 OK
- 201 Created
- 204 No Content
- 404 Not Found

### Common Mistake

❌ Every controller method must return `ResponseEntity`.

✅ It is optional but recommended when you need control over the response.

### Repository Reference

- `docs/13-exception-handling.md`

---

## Q74. What is content negotiation in Spring Boot?

### Answer

Content Negotiation is the process of selecting the appropriate response format based on the client's request.

Most REST APIs return JSON.

However, Spring can also return:

- XML
- Plain Text
- Other supported media types

The client specifies the desired format using the `Accept` header.

Example

```
Accept: application/json
```

### Additional Notes

Spring uses `HttpMessageConverter` to generate the appropriate response format.

### Common Mistake

❌ Spring Boot always returns JSON.

✅ JSON is the default for most REST APIs, but Spring supports multiple media types.

### Repository Reference

- `docs/03-dispatcherservlet.md`

---

## Q75. What are REST API best practices for Controllers?

### Answer

A good REST Controller should:

- Keep methods small and focused.
- Delegate business logic to the Service layer.
- Validate input using `@Valid`.
- Return DTOs instead of Entities.
- Use appropriate HTTP status codes.
- Use meaningful endpoint names.
- Handle exceptions globally.

Example

```
GET    /students

GET    /students/{id}

POST   /students

PUT    /students/{id}

DELETE /students/{id}
```

### Additional Notes

Controllers should coordinate requests and responses—not implement business logic.

### Common Mistake

❌ Writing business logic or SQL inside Controllers.

✅ Controllers should only handle HTTP communication and delegate work to Services.

### Repository Reference

- `StudentController.java`
- `docs/07-controller-layer.md`

# Section 9 — Service Layer & Repository Layer

---

## Q76. What is the purpose of the Service Layer?

### Answer

The Service Layer contains the application's business logic.

It acts as a bridge between the Controller and the Repository.

```
HTTP Request

↓

Controller

↓

Service

↓

Repository

↓

Database
```

Controllers handle HTTP requests.

Repositories handle database operations.

Services implement business rules.

### Additional Notes

Examples of business logic include:

- Validating business rules
- Processing payments
- Sending notifications
- Coordinating multiple repositories
- Applying transactions

### Common Mistake

❌ Writing business logic inside Controllers.

✅ Controllers should delegate business logic to the Service layer.

### Repository Reference

- `StudentService.java`
- `docs/08-service-layer.md`

---

## Q77. Why do we need a Service Layer?

### Answer

Without a Service Layer, Controllers become responsible for:

- Validation
- Business rules
- Database coordination
- Transactions

This violates the Single Responsibility Principle.

Using a Service Layer keeps responsibilities separated.

```
Controller

↓

Service

↓

Repository
```

### Additional Notes

A Service can coordinate multiple repositories.

Example

```
OrderRepository

↓

InventoryRepository

↓

PaymentRepository
```

A Controller should never manage this complexity.

### Common Mistake

❌ Service Layer is optional.

✅ While technically optional, it is considered a best practice in almost all production Spring Boot applications.

### Repository Reference

- `docs/08-service-layer.md`

---

## Q78. What is the Repository Layer?

### Answer

The Repository Layer is responsible for interacting with the database.

It abstracts persistence logic from the rest of the application.

Example

```java
@Repository
public interface StudentRepository
        extends JpaRepository<Student, Long> {

}
```

### Additional Notes

The Repository should only perform data access.

Business logic belongs in the Service layer.

### Common Mistake

❌ Repositories should contain business logic.

✅ Repositories should focus only on persistence.

### Repository Reference

- `StudentRepository.java`
- `docs/09-repository-layer.md`

---

## Q79. What is Spring Data JPA?

### Answer

Spring Data JPA is a Spring project that simplifies database access.

Instead of writing DAO implementations manually,

developers create interfaces such as:

```java
public interface StudentRepository
        extends JpaRepository<Student, Long> {

}
```

Spring automatically generates the implementation at runtime.

### Additional Notes

Spring Data JPA works together with Hibernate.

```
Repository

↓

Hibernate

↓

Database
```

### Common Mistake

❌ Spring Data JPA replaces Hibernate.

✅ Spring Data JPA uses Hibernate (or another JPA provider) to perform persistence.

### Repository Reference

- `docs/09-repository-layer.md`

---

## Q80. What is JpaRepository?

### Answer

`JpaRepository` is the most commonly used Spring Data interface.

It provides ready-to-use methods such as:

- `findAll()`
- `findById()`
- `save()`
- `deleteById()`
- `count()`
- `existsById()`

Example

```java
public interface StudentRepository
        extends JpaRepository<Student, Long> {

}
```

### Additional Notes

Spring automatically creates the implementation during application startup.

### Common Mistake

❌ Developers must implement JpaRepository methods.

✅ Spring generates the implementation automatically.

### Repository Reference

- `StudentRepository.java`
- `docs/09-repository-layer.md`

---

## Q81. Difference between CrudRepository and JpaRepository?

### Answer

| CrudRepository | JpaRepository |
|----------------|---------------|
| Basic CRUD operations | CRUD + JPA-specific features |
| No pagination support | Pagination support |
| No batch operations | Batch operations |
| Minimal functionality | Rich functionality |

### Additional Notes

Most Spring Boot applications use `JpaRepository`.

### Common Mistake

❌ CrudRepository is outdated.

✅ CrudRepository is still supported but JpaRepository offers additional features.

### Repository Reference

- `docs/09-repository-layer.md`

---

## Q82. How does Hibernate fit into Spring Data JPA?

### Answer

When a repository method is called,

```
repository.findById(id)
```

Spring Data JPA delegates the request to Hibernate.

Hibernate then:

- Generates SQL
- Executes SQL
- Maps database rows to Java entities

Flow

```
Repository

↓

Hibernate

↓

Database
```

### Additional Notes

Hibernate is the default JPA implementation used by Spring Boot.

### Common Mistake

❌ Repository executes SQL directly.

✅ Hibernate generates and executes SQL on behalf of the Repository.

### Repository Reference

- `docs/09-repository-layer.md`

---

## Q83. What is an Entity?

### Answer

An Entity is a Java class that represents a database table.

Example

```java
@Entity
public class Student {

    @Id
    private Long id;

    private String name;

}
```

Each object represents one row in the table.

### Additional Notes

Entities are managed by Hibernate.

They should represent the persistence model, not the API model.

### Common Mistake

❌ Entities should be returned directly from Controllers.

✅ Controllers should return DTOs instead.

### Repository Reference

- `Student.java`
- `docs/10-dto-pattern.md`

---

## Q84. Why shouldn't Controllers access Repositories directly?

### Answer

Although technically possible,

it violates separation of concerns.

Correct architecture:

```
Controller

↓

Service

↓

Repository
```

Benefits:

- Better maintainability
- Easier testing
- Clear responsibilities
- Reusable business logic

### Additional Notes

Business rules often involve multiple repositories.

The Service Layer coordinates these operations.

### Common Mistake

❌ Controller → Repository is acceptable for simple CRUD applications.

✅ Even simple applications benefit from a proper layered architecture.

### Repository Reference

- `docs/08-service-layer.md`
- `docs/09-repository-layer.md`

---

## Q85. What is `@Transactional`?

### Answer

`@Transactional` ensures that multiple database operations execute as a single unit of work.

If one operation fails,

Spring rolls back the entire transaction.

Example

```java
@Transactional
public StudentDTO save(StudentDTO dto) {

    // Business Logic

}
```

### Additional Notes

Example

```
Save Student

↓

Create Audit Log

↓

Send Notification
```

If saving the student fails,

the transaction rolls back to maintain data consistency.

### Common Mistake

❌ `@Transactional` is only used for database inserts.

✅ It can be used for inserts, updates, deletes, and any business operation requiring transactional consistency.

### Repository Reference

- `docs/08-service-layer.md`

# Section 10 — DTO Pattern & Exception Handling

---

## Q86. What is a DTO?

### Answer

DTO stands for **Data Transfer Object**.

A DTO is a simple Java object used to transfer data between different layers of an application or between the server and the client.

Unlike Entities, DTOs are **not managed by JPA** and do not represent database tables.

Example

```java
public class StudentDTO {

    private Long id;

    private String name;

    private String email;

}
```

### Additional Notes

DTOs are commonly used for:

- API Requests
- API Responses
- Communication between layers

### Common Mistake

❌ DTOs should contain business logic.

✅ DTOs should only carry data.

### Repository Reference

- `StudentDTO.java`
- `docs/10-dto-pattern.md`

---

## Q87. Why should we use DTOs instead of Entities?

### Answer

Returning Entities directly exposes the application's database model to clients.

Using DTOs provides:

- Better security
- Loose coupling
- Smaller payloads
- Easier API versioning
- Better maintainability

Flow

```
Database

↓

Entity

↓

DTO

↓

Client
```

### Additional Notes

Suppose the Entity contains:

```java
password

createdAt

updatedAt

active
```

These fields should usually not be exposed to API consumers.

DTOs solve this problem.

### Common Mistake

❌ Returning Entities directly from Controllers.

✅ Convert Entities into DTOs before returning responses.

### Repository Reference

- `docs/10-dto-pattern.md`

---

## Q88. What is object mapping?

### Answer

Object Mapping is the process of converting one object into another.

Most commonly:

```
DTO

↓

Entity
```

and

```
Entity

↓

DTO
```

Example

```java
Student student = mapper.toEntity(dto);

StudentDTO response = mapper.toDTO(student);
```

### Additional Notes

Mapping keeps Controllers and Services clean.

Large projects often use libraries like:

- MapStruct
- ModelMapper

### Common Mistake

❌ Mapping should happen inside Controllers.

✅ Mapping should be handled by a dedicated Mapper class or the Service layer.

### Repository Reference

- `StudentMapper.java`
- `docs/10-dto-pattern.md`

---

## Q89. What is `@ControllerAdvice`?

### Answer

`@ControllerAdvice` provides centralized exception handling for all Controllers.

Instead of writing exception handling logic in every Controller,

one class handles exceptions for the entire application.

Example

```java
@ControllerAdvice
public class GlobalExceptionHandler {

}
```

### Additional Notes

It is commonly used in MVC applications.

For REST APIs,

`@RestControllerAdvice` is usually preferred.

### Common Mistake

❌ `@ControllerAdvice` automatically returns JSON.

✅ It can return views or response bodies depending on the method configuration.

### Repository Reference

- `docs/13-exception-handling.md`

---

## Q90. Difference between `@ControllerAdvice` and `@RestControllerAdvice`?

### Answer

| @ControllerAdvice | @RestControllerAdvice |
|-------------------|-----------------------|
| Used for MVC applications | Used for REST APIs |
| Can return Views | Returns JSON by default |
| May require `@ResponseBody` | Includes `@ResponseBody` automatically |

### Additional Notes

`@RestControllerAdvice` is equivalent to:

```java
@ControllerAdvice
@ResponseBody
```

Most Spring Boot REST APIs use `@RestControllerAdvice`.

### Common Mistake

❌ Both annotations behave exactly the same.

✅ `@RestControllerAdvice` is specifically designed for REST APIs.

### Repository Reference

- `GlobalExceptionHandler.java`
- `docs/13-exception-handling.md`

---

## Q91. What is `@ExceptionHandler`?

### Answer

`@ExceptionHandler` maps a specific exception to a handler method.

Example

```java
@ExceptionHandler(StudentNotFoundException.class)
public ResponseEntity<ErrorResponse>
handleStudentNotFound(
        StudentNotFoundException ex){

}
```

Whenever the specified exception occurs,

Spring automatically invokes the handler.

### Additional Notes

Different exception types can have different handler methods.

### Common Mistake

❌ `@ExceptionHandler` catches every exception automatically.

✅ It only handles the exception types explicitly specified.

### Repository Reference

- `GlobalExceptionHandler.java`
- `docs/13-exception-handling.md`

---

## Q92. Why should we create custom exceptions?

### Answer

Custom exceptions represent business-specific errors.

Example

```java
StudentNotFoundException
```

is much more meaningful than

```java
RuntimeException
```

Benefits:

- Better readability
- Better error handling
- Cleaner business logic
- Easier debugging

### Additional Notes

Services typically throw custom exceptions.

The Global Exception Handler converts them into appropriate HTTP responses.

### Common Mistake

❌ Throw `Exception` or `RuntimeException` everywhere.

✅ Create meaningful exception classes that represent business scenarios.

### Repository Reference

- `StudentNotFoundException.java`
- `docs/13-exception-handling.md`

---

## Q93. What is `ResponseEntity`?

### Answer

`ResponseEntity` represents the complete HTTP response.

It provides control over:

- Status Code
- Headers
- Response Body

Example

```java
return ResponseEntity
        .status(HttpStatus.CREATED)
        .body(studentDTO);
```

### Additional Notes

Typical status codes include:

- 200 OK
- 201 Created
- 204 No Content
- 400 Bad Request
- 404 Not Found
- 500 Internal Server Error

### Common Mistake

❌ `ResponseEntity` is mandatory for every Controller.

✅ Use it when you need explicit control over the HTTP response.

### Repository Reference

- `docs/13-exception-handling.md`

---

## Q94. How does Spring handle validation errors?

### Answer

When a request object is annotated with `@Valid`,

Spring automatically validates it before the Controller method executes.

Example

```java
@PostMapping
public StudentDTO createStudent(

    @Valid
    @RequestBody StudentDTO dto){

}
```

If validation fails,

Spring throws:

```
MethodArgumentNotValidException
```

The Global Exception Handler converts it into a structured error response.

### Additional Notes

This avoids manual validation logic inside Controllers.

### Common Mistake

❌ Validation should always be written manually.

✅ Use Bean Validation annotations whenever possible.

### Repository Reference

- `docs/13-exception-handling.md`

---

## Q95. How would you design a production-ready API error response?

### Answer

A consistent error response should include:

```json
{
    "timestamp": "2026-08-07T12:30:45",
    "status": 404,
    "error": "Not Found",
    "message": "Student not found with id : 10",
    "path": "/students/10"
}
```

Recommended fields:

- Timestamp
- HTTP Status
- Error
- Message
- Request Path

### Additional Notes

A consistent error format makes APIs easier to consume and debug.

Avoid exposing stack traces or internal implementation details.

### Common Mistake

❌ Returning different error formats for different Controllers.

✅ Standardize API error responses across the entire application using a Global Exception Handler.

### Repository Reference

- `ErrorResponse.java`
- `GlobalExceptionHandler.java`
- `docs/13-exception-handling.md`

# Section 11 — Scenario-Based & Production Questions

---

## Q96. Explain the complete request lifecycle using your project.

### Answer

In this project, the request lifecycle is as follows:

```
HTTP Client

↓

Embedded Tomcat

↓

LoggingFilter

↓

DispatcherServlet

↓

HandlerMapping

↓

HandlerAdapter

↓

RequestInterceptor (preHandle)

↓

StudentController

↓

StudentService

↓

StudentRepository

↓

Hibernate

↓

H2 Database

↓

Hibernate

↓

StudentRepository

↓

StudentService

↓

StudentController

↓

RequestInterceptor (postHandle)

↓

Jackson

↓

RequestInterceptor (afterCompletion)

↓

LoggingFilter

↓

HTTP Response
```

Each layer has a single responsibility:

- Filter → Request preprocessing
- DispatcherServlet → Request coordination
- Controller → HTTP handling
- Service → Business logic
- Repository → Database access
- Hibernate → SQL generation
- Jackson → JSON conversion

### Interview Tip

This is one of the highest-frequency Spring Boot interview questions.

Don't simply draw the diagram.

Explain the responsibility of every component.

### Repository Reference

- Entire Repository
- `docs/04-request-lifecycle.md`

---

## Q97. A request never reaches the Controller. How would you debug it?

### Answer

I would debug the request in the following order:

```
1. Is the application running?

↓

2. Is the correct URL being called?

↓

3. Is the HTTP method correct?

↓

4. Does the request reach Tomcat?

↓

5. Check Filter logs.

↓

6. Check Interceptor logs.

↓

7. Verify Request Mapping.

↓

8. Check application logs.

↓

9. Enable Spring MVC debug logging.
```

### Additional Notes

The request may fail because of:

- Wrong URL
- Wrong HTTP method
- Filter rejecting the request
- Interceptor returning `false`
- Missing Controller mapping

### Common Mistake

❌ Immediately debugging the Controller.

✅ Verify whether the request reaches the Controller first.

---

## Q98. When would you choose a Filter instead of an Interceptor?

### Answer

Use a Filter when the logic should execute before Spring MVC.

Examples:

- CORS
- Character Encoding
- Compression
- Request Logging
- Security Headers

Filters are independent of Spring MVC.

### Interview Tip

If the logic does not require controller information,

choose a Filter.

### Repository Reference

- `LoggingFilter.java`
- `docs/05-filter.md`

---

## Q99. When would you choose an Interceptor instead of a Filter?

### Answer

Use an Interceptor when the logic depends on Spring MVC.

Examples:

- Authentication
- Authorization
- Audit Logging
- Measuring Controller execution time
- Accessing HandlerMethod information

### Additional Notes

Interceptors know:

- Which Controller executes
- Which Method executes
- Which Annotations are present

Filters do not.

### Repository Reference

- `RequestInterceptor.java`
- `docs/06-interceptor.md`

---

## Q100. Why shouldn't Controllers contain business logic?

### Answer

Controllers should only:

- Receive requests
- Validate input
- Delegate to Services
- Return responses

Business logic belongs in the Service layer because it:

- Improves maintainability
- Promotes reuse
- Simplifies testing
- Keeps responsibilities separate

### Common Mistake

❌ Writing validation, calculations and database logic inside Controllers.

✅ Keep Controllers thin.

### Repository Reference

- `docs/07-controller-layer.md`
- `docs/08-service-layer.md`

---

## Q101. Why shouldn't Controllers return Entities directly?

### Answer

Returning Entities exposes the database model to API consumers.

Problems include:

- Sensitive fields may be exposed
- Tight coupling
- Difficult API versioning
- Larger payloads

Correct approach

```
Entity

↓

Mapper

↓

DTO

↓

Client
```

### Repository Reference

- `StudentMapper.java`
- `docs/10-dto-pattern.md`

---

## Q102. How would you improve this project for production?

### Answer

Some production improvements include:

- JWT Authentication
- Spring Security
- PostgreSQL instead of H2
- API documentation using Swagger/OpenAPI
- Centralized logging (SLF4J + Logback)
- Docker support
- Unit and Integration Tests
- Redis Caching
- Pagination & Sorting
- Monitoring with Spring Boot Actuator
- CI/CD Pipeline
- Rate Limiting

### Interview Tip

Interviewers appreciate answers that go beyond "it works" and show awareness of production requirements.

---

## Q103. How would you secure these APIs?

### Answer

A production application would typically use:

- Spring Security
- JWT Authentication
- Password Encryption (BCrypt)
- Role-Based Authorization
- HTTPS
- CORS Configuration
- Input Validation

Example Flow

```
Client

↓

JWT Token

↓

Spring Security Filter

↓

Authentication

↓

Authorization

↓

Controller
```

---

## Q104. How would you improve application performance?

### Answer

Possible optimizations include:

- Database indexing
- Pagination
- Caching (Redis)
- Avoiding N+1 queries
- DTO projections
- Connection pooling (HikariCP)
- Lazy loading where appropriate
- Efficient logging
- Query optimization

### Additional Notes

Performance improvements should be based on profiling and measurement rather than assumptions.

---

## Q105. What are the strengths of this repository?

### Answer

This repository demonstrates:

- Spring Boot Architecture
- Request Lifecycle
- DispatcherServlet
- Filters
- Interceptors
- REST Controllers
- Service Layer
- Repository Layer
- Spring Data JPA
- DTO Pattern
- Bean Lifecycle
- IoC & Dependency Injection
- Global Exception Handling

Unlike a simple CRUD project,

it focuses on explaining how Spring Boot works internally.

### Interview Tip

When discussing this project, emphasize that its purpose is educational and architectural, demonstrating framework internals rather than business features.

---

# Final Advice for Interviews

When answering Spring Boot interview questions:

- Explain concepts in the correct execution order.
- Relate your answers to code you have actually written.
- Use diagrams where appropriate.
- Avoid memorized definitions without understanding.
- Be prepared to explain "why" in addition to "what".
- Use your repository to demonstrate practical implementation.

---

# Repository Revision Checklist

Before an interview, ensure you can confidently explain:

- Spring Framework vs Spring Boot
- Spring Boot Startup Sequence
- IoC & Dependency Injection
- Bean Lifecycle
- DispatcherServlet
- Complete Request Lifecycle
- Filters vs Interceptors
- REST Controllers
- Service Layer
- Repository Layer
- Spring Data JPA
- DTO Pattern
- Exception Handling
- Complete request flow using this project
