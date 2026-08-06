# Complete Spring Boot Request Lifecycle

## Learning Objectives

After completing this chapter, you should be able to:

- Explain the complete Spring Boot request lifecycle.
- Describe every component involved in request processing.
- Explain request flow from HTTP client to database.
- Explain response flow from database to HTTP response.
- Understand where Filters, Interceptors, DispatcherServlet and Jackson fit into the lifecycle.
- Answer one of the most common Spring Boot interview questions confidently.

---

# Introduction

Every HTTP request processed by a Spring Boot application follows a well-defined lifecycle.

Although developers usually write only Controllers, Services and Repositories, many framework components participate before and after the controller executes.

Understanding this lifecycle is essential for backend development, debugging and interview preparation.

---

# Complete Request Lifecycle

```
Client

↓

HTTP Request

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

↓

Client
```

---

# Step 1 — Client Sends Request

Example

```
GET /students/1
```

The browser, Postman or another application sends an HTTP request.

The request reaches the server hosting the Spring Boot application.

---

# Step 2 — Embedded Tomcat

Spring Boot contains an Embedded Tomcat server.

Tomcat accepts the incoming HTTP request.

It does not execute controllers directly.

Instead,

Tomcat forwards every request to

```
DispatcherServlet
```

---

# Step 3 — Servlet Filter

Before the request enters Spring MVC,

Servlet Filters execute.

Example

```
LoggingFilter
```

Typical use cases

- Logging
- Security
- Request timing
- Compression
- CORS

Execution

```
Request

↓

LoggingFilter

↓

DispatcherServlet
```

---

# Step 4 — DispatcherServlet

DispatcherServlet is the Front Controller of Spring MVC.

Every request enters Spring MVC through DispatcherServlet.

Responsibilities

- Receive requests
- Coordinate request processing
- Locate controller
- Handle exceptions
- Prepare response

---

# Step 5 — HandlerMapping

DispatcherServlet asks

```
Which controller should handle this request?
```

HandlerMapping searches all request mappings.

Example

```
GET /students/1

↓

StudentController

↓

getStudentById()
```

---

# Step 6 — HandlerAdapter

HandlerAdapter executes the selected controller method.

Responsibilities

- Resolve @PathVariable
- Resolve @RequestBody
- Resolve @RequestParam
- Validation
- Invoke controller

Example

```java
@GetMapping("/{id}")
public StudentDTO getStudent(
        @PathVariable Long id){
}
```

The value of `id` is automatically provided by Spring.

---

# Step 7 — Interceptor (preHandle)

Before the controller executes,

registered interceptors receive the request.

Example

```java
preHandle()
```

Typical uses

- Authentication
- Authorization
- Logging
- Request tracking

Execution

```
DispatcherServlet

↓

preHandle()

↓

Controller
```

---

# Step 8 — Controller

The controller receives the request.

Responsibilities

- Accept HTTP request
- Validate input
- Delegate business logic

Example

```java
@GetMapping("/{id}")
public StudentDTO getStudentById(Long id){

    return service.findById(id);

}
```

The controller should contain minimal business logic.

---

# Step 9 — Service Layer

The Service layer implements business logic.

Responsibilities

- Validation
- Business rules
- Transactions
- Coordination

Example

```
StudentService

↓

findById()
```

---

# Step 10 — Repository Layer

Repository communicates with the persistence layer.

Example

```java
studentRepository.findById(id);
```

Spring Data JPA delegates the operation to Hibernate.

---

# Step 11 — Hibernate

Hibernate converts repository operations into SQL.

Example

```sql
select *

from student

where id = 1;
```

Hibernate sends SQL to the database.

---

# Step 12 — Database

The database executes the SQL query.

Result

```
Student Record
```

is returned.

---

# Step 13 — Response Journey

The response returns through

```
Database

↓

Hibernate

↓

Repository

↓

Service

↓

Controller
```

No direct communication occurs between the database and the controller.

---

# Step 14 — Interceptor (postHandle)

After controller execution,

Spring calls

```
postHandle()
```

Typical uses

- Logging
- Modifying Model
- Metrics

---

# Step 15 — HttpMessageConverter

The controller returns

```java
StudentDTO
```

Spring selects

```
MappingJackson2HttpMessageConverter
```

Jackson converts

```
StudentDTO

↓

JSON
```

Example

```json
{
  "id":1,
  "name":"John",
  "email":"john@example.com"
}
```

---

# Step 16 — Interceptor (afterCompletion)

After the response has been written,

Spring executes

```
afterCompletion()
```

Typical uses

- Resource cleanup
- Performance measurement
- Final logging

---

# Step 17 — Servlet Filter

Control returns to the Filter.

Example

```
LoggingFilter

↓

Response Time

↓

Return Response
```

---

# Step 18 — HTTP Response

Tomcat sends the completed HTTP response back to the client.

The request lifecycle is complete.

---

# Visual Flow

```
HTTP Client

↓

Embedded Tomcat

↓

Logging Filter

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

Jackson

↓

Interceptor (afterCompletion)

↓

Logging Filter

↓

HTTP Response
```

---

# Execution Order

| Step | Component |
|------|-----------|
| 1 | Client |
| 2 | Embedded Tomcat |
| 3 | Filter |
| 4 | DispatcherServlet |
| 5 | HandlerMapping |
| 6 | HandlerAdapter |
| 7 | Interceptor (preHandle) |
| 8 | Controller |
| 9 | Service |
| 10 | Repository |
| 11 | Hibernate |
| 12 | Database |
| 13 | Repository |
| 14 | Service |
| 15 | Controller |
| 16 | Interceptor (postHandle) |
| 17 | Jackson |
| 18 | Interceptor (afterCompletion) |
| 19 | Filter |
| 20 | Client |

---

# Real-world Example

Request

```
GET /students/1
```

Execution

```
Postman

↓

Tomcat

↓

DispatcherServlet

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

StudentDTO

↓

Jackson

↓

JSON

↓

Postman
```

---

# Common Misconceptions

❌ Tomcat calls controllers directly.

Correct:

Tomcat forwards requests to DispatcherServlet.

---

❌ Filters are part of Spring MVC.

Correct:

Filters belong to the Servlet container and execute before Spring MVC.

---

❌ Repository executes SQL directly.

Correct:

Hibernate generates and executes SQL for Spring Data JPA repositories.

---

❌ Jackson converts JSON before the controller returns.

Correct:

Jackson serializes the controller's return value after controller execution.

---

# Summary

A Spring Boot request passes through multiple framework components before reaching business logic.

DispatcherServlet coordinates the request, HandlerMapping locates the controller, HandlerAdapter invokes it, Services execute business logic, Repositories interact with the database, and Jackson converts Java objects into JSON before the response is returned.

Understanding this lifecycle is essential for debugging, performance optimization and backend interviews.

---

# Key Takeaways

- Every request enters through Embedded Tomcat.
- DispatcherServlet is the Front Controller.
- HandlerMapping selects the controller.
- HandlerAdapter invokes controller methods.
- Filters execute before Spring MVC.
- Interceptors execute around controller methods.
- Services contain business logic.
- Repositories delegate persistence to Hibernate.
- Jackson serializes Java objects into JSON.
- The response follows the reverse path back to the client.

---

# Next Chapter

**05-filter.md**