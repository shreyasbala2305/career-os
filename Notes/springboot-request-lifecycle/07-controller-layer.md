# Controller Layer

## Learning Objectives

After completing this chapter, you should be able to:

- Explain the purpose of the Controller layer.
- Understand `@RestController`.
- Explain `@RequestMapping`.
- Use HTTP mapping annotations.
- Understand request parameter binding.
- Explain why controllers should remain thin.
- Identify controller best practices.

---

# Introduction

The Controller layer is responsible for receiving HTTP requests, validating input and delegating business logic to the Service layer.

Controllers act as the entry point of the application.

They should never contain complex business logic.

---

# Position in Spring Boot Architecture

```
HTTP Request

↓

Filter

↓

DispatcherServlet

↓

Interceptor

↓

Controller

↓

Service

↓

Repository

↓

Database
```

---

# What is a Controller?

A Controller is a Spring-managed component responsible for handling HTTP requests.

It receives requests from the DispatcherServlet and returns responses to the client.

Example

```java
@RestController
@RequestMapping("/students")
public class StudentController {

}
```

---

# @RestController

`@RestController` is a convenience annotation that combines:

```java
@Controller
```

and

```java
@ResponseBody
```

It tells Spring to return the method result directly as the HTTP response body.

Example

```java
@RestController
public class StudentController {

}
```

---

# @RequestMapping

`@RequestMapping` defines the base URL for a controller or maps specific requests.

Example

```java
@RequestMapping("/students")
```

All endpoints inside this controller will begin with:

```
/students
```

---

# HTTP Mapping Annotations

Spring provides specialized annotations for different HTTP methods.

| Annotation | HTTP Method |
|------------|-------------|
| `@GetMapping` | GET |
| `@PostMapping` | POST |
| `@PutMapping` | PUT |
| `@DeleteMapping` | DELETE |
| `@PatchMapping` | PATCH |

---

# GET Request

Retrieve resources.

Example

```java
@GetMapping
public List<StudentDTO> getAllStudents() {

    return service.findAll();

}
```

Request

```
GET /students
```

---

# GET by ID

```java
@GetMapping("/{id}")
public StudentDTO getStudentById(
        @PathVariable Long id) {

    return service.findById(id);

}
```

Request

```
GET /students/1
```

---

# POST Request

Create a resource.

Example

```java
@PostMapping
public StudentDTO createStudent(
        @RequestBody StudentDTO dto) {

    return service.save(dto);

}
```

Request

```
POST /students
```

---

# PUT Request

Update an existing resource.

```java
@PutMapping("/{id}")
public StudentDTO updateStudent(
        @PathVariable Long id,
        @RequestBody StudentDTO dto) {

    return service.update(id, dto);

}
```

---

# DELETE Request

Delete a resource.

```java
@DeleteMapping("/{id}")
public void deleteStudent(
        @PathVariable Long id) {

    service.delete(id);

}
```

---

# @PathVariable

Used to extract values from the URL.

Example

```
GET /students/10
```

```java
@PathVariable Long id
```

Result

```
id = 10
```

---

# @RequestBody

Converts JSON request bodies into Java objects using Jackson.

Example Request

```json
{
  "name":"John",
  "email":"john@example.com"
}
```

Controller

```java
@RequestBody StudentDTO dto
```

Spring automatically converts JSON into a `StudentDTO`.

---

# @Valid

Triggers Bean Validation before the controller method executes.

Example

```java
@PostMapping
public StudentDTO createStudent(

    @Valid
    @RequestBody StudentDTO dto){

}
```

If validation fails,

Spring throws a `MethodArgumentNotValidException`.

---

# Controller Responsibilities

A Controller should:

- Receive HTTP requests
- Validate input
- Call the Service layer
- Return HTTP responses

A Controller should **not**:

- Write SQL
- Implement business rules
- Access the database directly
- Create repositories manually

---

# Thin Controller Principle

Good Controller

```java
@GetMapping("/{id}")
public StudentDTO getStudent(

@PathVariable Long id){

    return service.findById(id);

}
```

Bad Controller

```java
@GetMapping("/{id}")
public StudentDTO getStudent(Long id){

    // SQL

    // Validation

    // Business Logic

    // Calculations

}
```

Business logic belongs in the Service layer.

---

# Request Processing

```
HTTP Request

↓

DispatcherServlet

↓

Controller

↓

Service

↓

Response
```

---

# Real-world Example

Request

```
GET /students/1
```

Execution

```
DispatcherServlet

↓

StudentController

↓

StudentService

↓

StudentRepository

↓

Database
```

---

# Best Practices

- Keep controllers thin.
- Delegate business logic to services.
- Use DTOs instead of entities.
- Validate request data.
- Return meaningful HTTP responses.
- Use constructor injection.

---

# Common Mistakes

❌ Writing business logic inside controllers.

Correct:

Move business logic to the Service layer.

---

❌ Returning entities directly.

Correct:

Return DTOs.

---

❌ Accessing repositories directly.

Correct:

Controllers should communicate only with the Service layer.

---

❌ Creating services using `new`.

Correct:

Use Dependency Injection.

---

# Summary

Controllers receive HTTP requests, delegate work to the Service layer and return HTTP responses.

They should remain lightweight and focus only on request handling and response generation.

---

# Key Takeaways

- Controllers are the entry point of the application.
- `@RestController` creates REST APIs.
- `@RequestMapping` defines request paths.
- `@RequestBody` converts JSON into Java objects.
- `@PathVariable` extracts URL values.
- Controllers should remain thin and delegate business logic.

---

# Interview Questions

### What is a Controller?

A Controller is a Spring MVC component responsible for handling HTTP requests and returning HTTP responses.

---

### Difference between `@Controller` and `@RestController`?

- `@Controller` is used for MVC applications that return views.
- `@RestController` is used for REST APIs and returns response bodies directly.

---

### Why should controllers remain thin?

Controllers should only handle requests and responses. Business logic belongs in the Service layer, improving maintainability and testability.

---

### What does `@RequestBody` do?

It uses Jackson to convert the JSON request body into a Java object.

---

### What does `@PathVariable` do?

It binds values from the request URL to method parameters.

---

# Next Chapter

**08-service-layer.md**