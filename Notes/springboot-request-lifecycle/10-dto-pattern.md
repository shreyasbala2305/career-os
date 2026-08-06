# DTO Pattern

## Learning Objectives

After completing this chapter, you should be able to:

- Explain what a DTO is.
- Explain why DTOs exist.
- Differentiate Entity and DTO.
- Explain object mapping.
- Understand why entities should not be exposed.
- Understand request and response DTOs.
- Follow DTO best practices.

---

# Introduction

DTO stands for **Data Transfer Object**.

A DTO is a simple Java object used to transfer data between different layers of an application or between the server and the client.

Unlike JPA entities, DTOs are **not** directly mapped to database tables.

They exist only to exchange data.

---

# Position in Spring Boot Architecture

```
HTTP Request

↓

Request DTO

↓

Controller

↓

Service

↓

Entity

↓

Repository

↓

Database

↓

Entity

↓

Service

↓

Response DTO

↓

Jackson

↓

JSON Response
```

---

# What is a DTO?

A DTO is a plain Java object that contains only the data required for communication.

Example

```java
public class StudentDTO {

    private Long id;

    private String name;

    private String email;

}
```

A DTO contains no persistence logic.

---

# Why do we need DTOs?

Without DTOs

```
Database

↓

Entity

↓

Client
```

Every database field becomes visible.

With DTOs

```
Database

↓

Entity

↓

DTO

↓

Client
```

Only required data is exposed.

---

# Entity vs DTO

| Entity | DTO |
|---------|-----|
| Database Object | Data Transfer Object |
| Managed by JPA | Not managed by JPA |
| Contains persistence mapping | Contains only required fields |
| Represents database table | Represents API request/response |
| Used internally | Used externally |

---

# Example Entity

```java
@Entity
public class Student {

    @Id
    private Long id;

    private String name;

    private String email;

}
```

---

# Example DTO

```java
public class StudentDTO {

    private Long id;

    private String name;

    private String email;

}
```

Notice that the DTO contains only the data required by the API.

---

# Request DTO

Used when data comes **from the client**.

Example

```
POST /students
```

Request

```json
{
    "name":"John",
    "email":"john@example.com"
}
```

Spring converts JSON into

```java
StudentDTO
```

using Jackson.

---

# Response DTO

Used when data goes **to the client**.

Example

```
Student Entity

↓

StudentDTO

↓

JSON
```

Response

```json
{
    "id":1,
    "name":"John",
    "email":"john@example.com"
}
```

---

# Why not return Entity directly?

Suppose the entity becomes

```java
@Entity
public class Student {

    private Long id;

    private String name;

    private String email;

    private String password;

    private LocalDateTime createdAt;

    private boolean active;

}
```

Returning the entity exposes

```
password

createdAt

active
```

to every client.

This is both a **security** and **design** problem.

A DTO allows only the required fields to be returned.

---

# Object Mapping

DTOs and Entities need conversion.

```
StudentDTO

↓

Mapper

↓

Student Entity
```

and

```
Student Entity

↓

Mapper

↓

StudentDTO
```

---

# StudentMapper Example

```java
@Component
public class StudentMapper {

    public StudentDTO toDTO(Student student){

        return StudentDTO.builder()
                .id(student.getId())
                .name(student.getName())
                .email(student.getEmail())
                .build();

    }

    public Student toEntity(StudentDTO dto){

        return Student.builder()
                .id(dto.getId())
                .name(dto.getName())
                .email(dto.getEmail())
                .build();

    }

}
```

---

# Request Flow

```
JSON

↓

Jackson

↓

StudentDTO

↓

Mapper

↓

Entity

↓

Service

↓

Repository
```

---

# Response Flow

```
Repository

↓

Entity

↓

Mapper

↓

StudentDTO

↓

Jackson

↓

JSON
```

---

# Benefits of DTOs

- Hide sensitive fields
- Reduce payload size
- Improve API security
- Separate database model from API model
- Version APIs more easily
- Keep entities focused on persistence

---

# Best Practices

- Never expose entities directly.
- Use separate Request and Response DTOs for complex applications.
- Keep DTOs lightweight.
- Perform mapping in a dedicated Mapper class.
- Validate request DTOs using Bean Validation.

---

# Real-world Example

Database Entity

```java
Student

id

name

email

password

createdAt

updatedAt
```

Response DTO

```java
StudentResponseDTO

id

name

email
```

The client receives only the required information.

---

# Common Mistakes

❌ Returning entities directly from controllers.

Correct:

Return DTOs.

---

❌ Putting business logic inside DTOs.

Correct:

DTOs should only transfer data.

---

❌ Using entities as request models.

Correct:

Use Request DTOs.

---

❌ Performing database operations inside Mapper classes.

Correct:

Mappers should only convert objects.

---

# Summary

DTOs provide a clean separation between the application's persistence model and its API model.

They improve security, maintainability and flexibility while preventing unnecessary database fields from being exposed.

---

# Key Takeaways

- DTO stands for Data Transfer Object.
- DTOs transfer data between layers.
- Entities represent database tables.
- DTOs represent API requests and responses.
- Mapping converts Entities to DTOs and vice versa.
- DTOs improve security and maintainability.

---

# Interview Questions

### What is a DTO?

A DTO (Data Transfer Object) is a simple object used to transfer data between different layers of an application or between the server and the client.

---

### Why do we use DTOs?

To hide internal database structures, expose only required data, improve security and separate the persistence model from the API model.

---

### Difference between Entity and DTO?

- Entity represents a database table.
- DTO represents data exchanged through the API.

---

### Why shouldn't we return entities directly?

Entities may expose sensitive fields, create tight coupling between the database and API, and make future changes more difficult.

---

### Where should object mapping happen?

Object mapping should be performed in a dedicated Mapper class or a mapping framework such as MapStruct.

---

# Next Chapter

**11-bean-lifecycle.md**