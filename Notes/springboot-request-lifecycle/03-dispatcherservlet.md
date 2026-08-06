# DispatcherServlet & Spring MVC Internals

## Learning Objectives

After completing this chapter, you should be able to:

- Explain DispatcherServlet.
- Explain the Front Controller Pattern.
- Explain HandlerMapping.
- Explain HandlerAdapter.
- Explain HttpMessageConverter.
- Explain the complete Spring MVC request processing flow.
- Describe how DispatcherServlet coordinates request handling.

---

# Introduction

DispatcherServlet is the central component of Spring MVC.

Every incoming HTTP request first reaches the DispatcherServlet.

DispatcherServlet is responsible for:

- Receiving requests
- Finding the correct controller
- Executing the controller
- Processing the response
- Returning the HTTP response to the client

It acts as the **Front Controller** of Spring MVC.

---

# What is DispatcherServlet?

DispatcherServlet is the central request dispatcher in Spring MVC.

Instead of every controller handling requests independently,

all requests pass through DispatcherServlet first.

```
HTTP Request

↓

DispatcherServlet

↓

Controller

↓

HTTP Response
```

---

# Why do we need DispatcherServlet?

Without DispatcherServlet,

every controller would have to handle routing, request processing and response generation independently.

Spring centralizes these responsibilities inside DispatcherServlet.

Benefits:

- Centralized request handling
- Better maintainability
- Extensible processing pipeline
- Consistent request processing

---

# Front Controller Pattern

Spring MVC follows the Front Controller Pattern.

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

Every controller receives requests independently.

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

DispatcherServlet becomes the single entry point.

---

# Complete Request Processing Flow

```
HTTP Client

↓

Embedded Tomcat

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

DispatcherServlet

↓

Embedded Tomcat

↓

HTTP Response
```

---

# Embedded Tomcat

The request first reaches the embedded Tomcat server.

Tomcat receives the HTTP request and forwards it to DispatcherServlet.

Tomcat never calls controllers directly.

---

# DispatcherServlet

DispatcherServlet receives every request.

Its responsibilities include:

- Identifying the correct controller
- Executing the controller
- Handling exceptions
- Processing responses
- Coordinating the Spring MVC workflow

---

# HandlerMapping

DispatcherServlet does not know which controller should process the request.

It asks the HandlerMapping.

Example

Request

```
GET /students/1
```

HandlerMapping searches all request mappings.

```
StudentController

↓

getStudentById()
```

The matching handler is returned to DispatcherServlet.

---

# HandlerAdapter

Once the controller method is identified,

DispatcherServlet delegates execution to HandlerAdapter.

HandlerAdapter is responsible for:

- Invoking the controller method
- Resolving method arguments
- Binding request parameters
- Resolving `@PathVariable`
- Resolving `@RequestBody`
- Triggering validation

Example

```java
@GetMapping("/{id}")
public StudentDTO getStudent(
        @PathVariable Long id) {
}
```

HandlerAdapter automatically provides the value of `id`.

---

# Interceptors

Before the controller executes,

registered interceptors receive the request.

Typical execution order:

```
preHandle()

↓

Controller

↓

postHandle()

↓

afterCompletion()
```

Interceptors are useful for:

- Authentication
- Authorization
- Logging
- Performance monitoring

---

# Controller

The controller handles the incoming request.

Example

```java
@GetMapping("/{id}")
public StudentDTO getStudentById(
        @PathVariable Long id) {

    return service.findById(id);

}
```

The controller delegates business logic to the service layer.

---

# Service Layer

The service layer contains business logic.

Responsibilities:

- Business rules
- Validation
- Transactions
- Coordination between components

---

# Repository Layer

Repositories communicate with the database.

Example

```java
studentRepository.findById(id);
```

Spring Data JPA delegates the operation to Hibernate, which generates SQL and interacts with the database.

---

# Database

The database executes the generated SQL and returns data to the repository.

The response then travels back through:

```
Repository

↓

Service

↓

Controller
```

---

# HttpMessageConverter

The controller returns a Java object.

Example

```java
StudentDTO
```

Spring selects an appropriate `HttpMessageConverter`.

For JSON responses,

the default converter is:

```
MappingJackson2HttpMessageConverter
```

Jackson converts:

```
StudentDTO

↓

JSON
```

---

# Response Flow

```
StudentDTO

↓

Jackson

↓

JSON

↓

DispatcherServlet

↓

Tomcat

↓

HTTP Response
```

---

# Sequence Diagram

```
Client
  │
  ▼
Tomcat
  │
  ▼
DispatcherServlet
  │
  ▼
HandlerMapping
  │
  ▼
HandlerAdapter
  │
  ▼
Interceptor
  │
  ▼
Controller
  │
  ▼
Service
  │
  ▼
Repository
  │
  ▼
Database
  │
  ▲
Repository
  │
  ▲
Service
  │
  ▲
Controller
  │
  ▼
HttpMessageConverter
  │
  ▼
DispatcherServlet
  │
  ▼
Tomcat
  │
  ▼
Client
```

---

# Real-world Example

Request:

```
GET /students/1
```

Execution:

```
Tomcat

↓

DispatcherServlet

↓

HandlerMapping

↓

StudentController

↓

StudentService

↓

StudentRepository

↓

H2 Database

↓

StudentDTO

↓

Jackson

↓

JSON Response
```

---

# Common Misconceptions

❌ Tomcat directly invokes controllers.

Correct:

Tomcat forwards requests to DispatcherServlet.

---

❌ DispatcherServlet executes controller methods directly.

Correct:

DispatcherServlet delegates controller execution to HandlerAdapter.

---

❌ Jackson executes before the controller.

Correct:

Jackson serializes the controller's return value after the controller method completes.

---

❌ HandlerMapping executes business logic.

Correct:

HandlerMapping only identifies the appropriate handler for a request.

---

# Summary

DispatcherServlet is the Front Controller of Spring MVC.

It coordinates the complete request lifecycle by working with HandlerMapping, HandlerAdapter, Interceptors, Controllers, Services, Repositories and HttpMessageConverters.

This architecture centralizes request processing and enables Spring MVC's flexibility and extensibility.

---

# Key Takeaways

- DispatcherServlet is the entry point of Spring MVC.
- Spring MVC follows the Front Controller Pattern.
- HandlerMapping locates the correct controller.
- HandlerAdapter invokes controller methods.
- Interceptors execute before and after controller methods.
- HttpMessageConverter converts Java objects into HTTP responses.
- Jackson is the default JSON converter in Spring Boot.

---

# Next Chapter

**04-request-lifecycle.md**