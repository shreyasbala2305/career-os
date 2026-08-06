# Service Layer

## Learning Objectives

After completing this chapter, you should be able to:

- Explain the purpose of the Service layer.
- Understand the `@Service` annotation.
- Explain why business logic belongs in the Service layer.
- Understand the interaction between Controllers and Repositories.
- Explain transactions (basic).
- Follow Service layer best practices.
- Answer common Service layer interview questions.

---

# Introduction

The Service layer acts as the bridge between the Controller and the Repository.

Its primary responsibility is to implement business logic.

Controllers should handle HTTP requests.

Repositories should handle database operations.

The Service layer coordinates these components.

---

# Position in Spring Boot Architecture

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

---

# What is a Service?

A Service is a Spring-managed component responsible for implementing business rules and coordinating application logic.

Example

```java
@Service
public class StudentService {

}
```

---

# Why do we need a Service Layer?

Without a Service layer, controllers become responsible for business rules.

Example

```
Controller

↓

Validation

↓

Business Logic

↓

Repository

↓

Database
```

This leads to tightly coupled and difficult-to-maintain code.

Using a Service layer separates responsibilities.

```
Controller

↓

Service

↓

Repository

↓

Database
```

---

# Responsibilities of the Service Layer

A Service should:

- Implement business logic
- Validate business rules
- Coordinate multiple repositories
- Handle transactions
- Prepare data for the Controller

A Service should **not**:

- Handle HTTP requests
- Return HTML or Views
- Write SQL queries directly

---

# @Service Annotation

The `@Service` annotation marks a class as a Service component.

Example

```java
@Service
public class StudentService {

}
```

Spring automatically detects it during component scanning and registers it as a Bean.

---

# Example Service

```java
@Service
@RequiredArgsConstructor
public class StudentService {

    private final StudentRepository repository;

    public List<StudentDTO> findAll() {

        return repository.findAll()
                .stream()
                .map(mapper::toDTO)
                .toList();

    }

}
```

The Controller simply delegates to the Service.

---

# Business Logic Example

Suppose a student must have a unique email address.

The Service should enforce this rule.

Example

```java
if(repository.existsByEmail(dto.getEmail())){

    throw new DuplicateEmailException();

}
```

This validation belongs in the Service, not the Controller.

---

# Service to Repository Flow

```
Controller

↓

StudentService

↓

StudentRepository

↓

Database
```

The Controller never accesses the Repository directly.

---

# Transactions (Basic)

Many business operations involve multiple database actions.

Example

```
Save Student

↓

Create Audit Record

↓

Send Notification
```

All these operations should either succeed together or fail together.

Spring provides transaction management using:

```java
@Transactional
```

---

# Example

```java
@Transactional
public StudentDTO save(StudentDTO dto){

    Student student = mapper.toEntity(dto);

    Student saved = repository.save(student);

    return mapper.toDTO(saved);

}
```

If an exception occurs, Spring rolls back the transaction automatically.

---

# Service Layer Best Practices

- Keep Controllers thin.
- Place business rules in Services.
- Use constructor injection.
- Return DTOs.
- Keep Services focused on one responsibility.
- Use transactions where appropriate.

---

# Real-world Example

Request

```
POST /students
```

Execution

```
Controller

↓

StudentService

↓

Validate Business Rules

↓

Repository

↓

Database

↓

StudentDTO

↓

Controller

↓

HTTP Response
```

---

# Common Mistakes

❌ Writing business logic inside Controllers.

Correct:

Move business rules to the Service layer.

---

❌ Accessing the database directly from Controllers.

Correct:

Controllers should communicate only with Services.

---

❌ Putting SQL inside Services.

Correct:

Repositories are responsible for database access.

---

❌ Creating Service objects manually using `new`.

Correct:

Use Spring Dependency Injection.

---

# Summary

The Service layer is responsible for implementing business logic and coordinating interactions between Controllers and Repositories.

It keeps Controllers lightweight and Repositories focused solely on data access.

---

# Key Takeaways

- The Service layer contains business logic.
- `@Service` marks a Spring-managed Service Bean.
- Controllers delegate work to Services.
- Services interact with Repositories.
- `@Transactional` ensures atomic database operations.
- Business validation belongs in the Service layer.

---

# Interview Questions

### What is the Service layer?

The Service layer contains the application's business logic and acts as an intermediary between Controllers and Repositories.

---

### Why do we need a Service layer?

To separate business logic from request handling and database access, making the application easier to maintain and test.

---

### Can a Controller access a Repository directly?

Technically yes, but it is not recommended.

Controllers should interact with the Service layer instead.

---

### What does `@Service` do?

It marks a class as a Spring Service Bean, allowing Spring to detect and manage it automatically.

---

### What is `@Transactional`?

`@Transactional` ensures that a group of database operations executes as a single transaction. If one operation fails, Spring rolls back the entire transaction.

---

# Next Chapter

**09-repository-layer.md**