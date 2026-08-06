# Exception Handling in Spring Boot

## Learning Objectives

After completing this chapter, you should be able to:

- Explain why exception handling is important.
- Explain local and global exception handling.
- Understand `@ExceptionHandler`.
- Understand `@ControllerAdvice` and `@RestControllerAdvice`.
- Explain custom exceptions.
- Understand validation exception handling.
- Design consistent API error responses.
- Answer common exception handling interview questions.

---

# Introduction

Exceptions are unexpected situations that occur during application execution.

Examples include:

- Student not found
- Invalid request data
- Database connection failure
- NullPointerException

Without proper handling, Spring Boot returns generic error responses that are not user-friendly.

Centralized exception handling provides clean, consistent, and maintainable API responses.

---

# What is Exception Handling?

Exception Handling is the process of detecting runtime errors and returning meaningful responses to the client.

Instead of exposing stack traces or internal details, applications should return structured error information.

Example

```
Request

↓

Controller

↓

Service

↓

Exception

↓

Global Exception Handler

↓

JSON Response
```

---

# Why do we need Global Exception Handling?

Without centralized exception handling:

```
Controller A

↓

try-catch

----------------

Controller B

↓

try-catch

----------------

Controller C

↓

try-catch
```

The same logic is duplicated across controllers.

With `@RestControllerAdvice`:

```
Controller A

↓

Controller B

↓

Controller C

↓

Global Exception Handler
```

All controllers share one centralized error handling mechanism.

---

# Local Exception Handling

Spring allows handling exceptions inside a controller.

Example

```java
@RestController
public class StudentController {

    @ExceptionHandler(StudentNotFoundException.class)
    public String handleStudentNotFound() {

        return "Student Not Found";

    }

}
```

Limitation

The handler applies only to that controller.

---

# Global Exception Handling

Global exception handling uses

```java
@RestControllerAdvice
```

Example

```java
@RestControllerAdvice
public class GlobalExceptionHandler {

}
```

Now every controller automatically uses this handler.

---

# @ExceptionHandler

`@ExceptionHandler` maps an exception to a handler method.

Example

```java
@ExceptionHandler(StudentNotFoundException.class)
public ResponseEntity<ErrorResponse>
handleStudentNotFound(
        StudentNotFoundException ex){

}
```

Whenever the specified exception is thrown,

Spring executes this method.

---

# Custom Exception

Example

```java
public class StudentNotFoundException
        extends RuntimeException {

    public StudentNotFoundException(Long id){

        super("Student not found with id : " + id);

    }

}
```

Instead of returning `null`,

the Service throws this exception.

---

# Throwing Custom Exceptions

Example

```java
Student student = repository.findById(id)
        .orElseThrow(() ->
                new StudentNotFoundException(id));
```

Spring automatically forwards the exception to the Global Exception Handler.

---

# Error Response Model

A consistent API error response improves usability.

Example

```java
public class ErrorResponse {

    private LocalDateTime timestamp;

    private int status;

    private String error;

    private String message;

    private String path;

}
```

Example JSON

```json
{
  "timestamp":"2026-08-07T15:10:00",
  "status":404,
  "error":"Not Found",
  "message":"Student not found with id : 100",
  "path":"/students/100"
}
```

---

# Handling Validation Errors

Request DTO

```java
public class StudentDTO {

    @NotBlank
    private String name;

    @Email
    private String email;

}
```

Controller

```java
@PostMapping
public StudentDTO createStudent(

        @Valid
        @RequestBody StudentDTO dto){

}
```

If validation fails,

Spring throws

```
MethodArgumentNotValidException
```

The Global Exception Handler converts it into a structured error response.

---

# Exception Handling Flow

```
HTTP Request

↓

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

Jackson

↓

JSON

↓

HTTP Response
```

---

# Common HTTP Status Codes

| Status | Meaning |
|---------|----------|
| 200 | OK |
| 201 | Created |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 409 | Conflict |
| 500 | Internal Server Error |

---

# ResponseEntity

`ResponseEntity` allows full control over the HTTP response.

Example

```java
return ResponseEntity
        .status(HttpStatus.NOT_FOUND)
        .body(response);
```

It controls:

- HTTP Status
- Headers
- Body

---

# Exception Handling in This Repository

```
Controller

↓

StudentService

↓

StudentNotFoundException

↓

GlobalExceptionHandler

↓

ErrorResponse

↓

JSON
```

---

# Real-world Example

Request

```
GET /students/100
```

Execution

```
Controller

↓

Service

↓

StudentNotFoundException

↓

@RestControllerAdvice

↓

404 Response

↓

Client
```

---

# Best Practices

- Use custom exceptions for business errors.
- Centralize exception handling.
- Return consistent error responses.
- Avoid exposing stack traces.
- Use meaningful HTTP status codes.
- Validate request DTOs.

---

# Common Mistakes

❌ Returning `null` when data is missing.

Correct:

Throw a custom exception.

---

❌ Using try-catch in every controller.

Correct:

Use `@RestControllerAdvice`.

---

❌ Returning HTTP 200 for every error.

Correct:

Return appropriate HTTP status codes.

---

❌ Exposing internal exception details.

Correct:

Return user-friendly error messages.

---

# Summary

Spring Boot provides a powerful exception handling mechanism using `@ExceptionHandler` and `@RestControllerAdvice`.

By centralizing exception handling, applications become easier to maintain and provide consistent, meaningful API responses.

---

# Key Takeaways

- Handle exceptions centrally.
- Use `@RestControllerAdvice`.
- Throw custom exceptions from the Service layer.
- Return structured error responses.
- Use appropriate HTTP status codes.
- Validate request DTOs using `@Valid`.

---

# Interview Questions

### What is `@RestControllerAdvice`?

`@RestControllerAdvice` is a specialization of `@ControllerAdvice` that provides global exception handling for REST controllers and automatically serializes responses as JSON.

---

### Difference between `@ControllerAdvice` and `@RestControllerAdvice`?

- `@ControllerAdvice` is used for MVC applications and may return views.
- `@RestControllerAdvice` combines `@ControllerAdvice` and `@ResponseBody`, making it ideal for REST APIs.

---

### Why use custom exceptions?

Custom exceptions make business errors explicit, improve readability, and allow different exception types to be handled separately.

---

### What is `@ExceptionHandler`?

`@ExceptionHandler` maps a specific exception type to a method that generates an appropriate response.

---

### Why use `ResponseEntity`?

`ResponseEntity` provides complete control over the HTTP response, including the status code, headers, and body.

---

### Where should exceptions be thrown?

Business exceptions should generally be thrown from the Service layer, where business rules are implemented.

---

# Next Chapter

**14-interview-handbook.md**