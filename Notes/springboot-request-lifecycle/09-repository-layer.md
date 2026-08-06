# Repository Layer

## Learning Objectives

After completing this chapter, you should be able to:

- Explain the purpose of the Repository layer.
- Understand Spring Data JPA.
- Explain `JpaRepository`.
- Differentiate `JpaRepository` and `CrudRepository`.
- Explain how Repositories interact with Hibernate.
- Understand the Repository → Database flow.
- Follow Repository best practices.
- Answer common Repository interview questions.

---

# Introduction

The Repository layer is responsible for interacting with the database.

Instead of writing SQL manually, Spring Data JPA provides repository interfaces that automatically implement common database operations.

The Repository layer should focus only on data access.

Business logic belongs in the Service layer.

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

Hibernate

↓

Database
```

---

# What is a Repository?

A Repository is a Spring-managed component responsible for performing CRUD operations and database access.

Example

```java
@Repository
public interface StudentRepository
        extends JpaRepository<Student, Long> {

}
```

---

# Why do we need a Repository?

Without a Repository

```
Service

↓

SQL

↓

JDBC

↓

Database
```

The Service layer becomes responsible for persistence.

With a Repository

```
Service

↓

Repository

↓

Database
```

Responsibilities are clearly separated.

---

# Spring Data JPA

Spring Data JPA simplifies data access by generating repository implementations automatically.

Instead of writing SQL for common operations, developers extend repository interfaces.

Example

```java
public interface StudentRepository
        extends JpaRepository<Student, Long> {

}
```

No implementation class is required.

Spring generates it at runtime.

---

# JpaRepository

`JpaRepository` is the most commonly used repository interface.

It extends:

```
Repository

↓

CrudRepository

↓

PagingAndSortingRepository

↓

JpaRepository
```

It provides CRUD operations, pagination, sorting and additional JPA-specific features.

---

# Common JpaRepository Methods

| Method | Purpose |
|---------|---------|
| `findAll()` | Retrieve all records |
| `findById()` | Retrieve by primary key |
| `save()` | Insert or update |
| `deleteById()` | Delete by ID |
| `existsById()` | Check existence |
| `count()` | Count records |

---

# CrudRepository vs JpaRepository

| CrudRepository | JpaRepository |
|----------------|---------------|
| Basic CRUD | CRUD + JPA Features |
| No Pagination | Pagination Support |
| No Batch Operations | Batch Operations |
| Limited Features | Most Common Choice |

---

# Repository Example

```java
@Repository
public interface StudentRepository
        extends JpaRepository<Student, Long> {

}
```

Spring automatically creates the implementation.

No manual class is required.

---

# Repository Flow

```
StudentService

↓

StudentRepository

↓

Hibernate

↓

Database
```

The Service communicates only with the Repository.

---

# Hibernate Integration

When the Repository executes

```java
repository.findById(id);
```

Spring Data JPA delegates the request to Hibernate.

Hibernate generates SQL similar to:

```sql
select
    id,
    name,
    email
from
    student
where
    id = 1;
```

Hibernate executes the SQL against the configured database.

---

# Entity Mapping

Repositories work with JPA entities.

Example

```java
@Entity
public class Student {

    @Id
    private Long id;

}
```

Hibernate maps the entity to the corresponding database table.

---

# Derived Query Methods

Spring Data JPA can generate queries based on method names.

Example

```java
Optional<Student> findByEmail(String email);
```

Spring automatically generates the SQL.

Another example

```java
List<Student> findByName(String name);
```

No SQL is required.

---

# Custom Queries

When derived queries are insufficient,

developers can use `@Query`.

Example

```java
@Query("SELECT s FROM Student s WHERE s.email = :email")
Optional<Student> findStudentByEmail(String email);
```

---

# Repository Best Practices

- Keep repositories focused on persistence.
- Avoid business logic inside repositories.
- Prefer derived query methods where possible.
- Use custom queries only when necessary.
- Return entities to the Service layer.
- Keep repositories small and focused.

---

# Real-world Example

Request

```
GET /students/1
```

Execution

```
Controller

↓

Service

↓

StudentRepository

↓

Hibernate

↓

H2 Database

↓

Student Entity

↓

Service

↓

DTO

↓

Controller
```

---

# Common Mistakes

❌ Writing business logic inside repositories.

Correct:

Repositories should only access data.

---

❌ Controllers calling repositories directly.

Correct:

Controllers should communicate through the Service layer.

---

❌ Returning DTOs directly from repositories.

Correct:

Repositories should return entities.

DTO mapping belongs in the Service or Mapper layer.

---

❌ Writing SQL for every query.

Correct:

Use derived query methods whenever possible.

---

# Summary

The Repository layer abstracts database access from the rest of the application.

Spring Data JPA automatically generates repository implementations, while Hibernate converts repository operations into SQL and interacts with the database.

This separation keeps persistence logic isolated and maintainable.

---

# Key Takeaways

- Repositories handle database access.
- `JpaRepository` provides CRUD, pagination and sorting.
- Spring generates repository implementations automatically.
- Hibernate executes SQL generated from repository operations.
- Business logic should never be placed in repositories.

---

# Interview Questions

### What is the Repository layer?

The Repository layer is responsible for interacting with the database and performing persistence operations.

---

### What is Spring Data JPA?

Spring Data JPA is a framework that simplifies data access by generating repository implementations automatically.

---

### What is `JpaRepository`?

`JpaRepository` is a Spring Data interface that provides CRUD operations, pagination, sorting and JPA-specific functionality.

---

### Difference between `CrudRepository` and `JpaRepository`?

- `CrudRepository` provides basic CRUD operations.
- `JpaRepository` extends it with pagination, sorting and additional JPA features.

---

### Does `JpaRepository` execute SQL directly?

No.

Spring Data JPA delegates persistence operations to Hibernate, which generates and executes SQL.

---

### Can a Repository contain business logic?

No.

Business logic belongs in the Service layer.

Repositories should focus only on data access.

---

# Next Chapter

**10-dto-pattern.md**