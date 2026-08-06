# Spring Bean Lifecycle

## Learning Objectives

After completing this chapter, you should be able to:

- Explain what a Spring Bean is.
- Explain the Bean Lifecycle.
- Understand Bean creation and destruction.
- Explain `@PostConstruct`.
- Explain `@PreDestroy`.
- Understand Bean scopes.
- Differentiate Singleton and Prototype Beans.
- Answer common Bean Lifecycle interview questions.

---

# Introduction

A Bean is an object that is created, configured and managed by the Spring IoC Container.

Unlike normal Java objects, Spring Beans have a complete lifecycle managed by Spring.

Example

```java
@Service
public class StudentService {

}
```

Spring automatically creates and manages this object.

---

# What is a Spring Bean?

A Spring Bean is any object managed by the Spring IoC Container.

Beans are usually created using annotations like:

- `@Component`
- `@Service`
- `@Repository`
- `@Controller`
- `@RestController`
- `@Configuration`

---

# Bean Lifecycle Overview

The lifecycle of a Bean follows these steps:

```
Component Scan

↓

Bean Creation

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

---

# Step 1 — Component Scanning

During application startup,

Spring scans packages for classes annotated with:

```
@Component

@Service

@Repository

@Controller

@RestController
```

Every discovered class becomes a candidate Bean.

---

# Step 2 — Bean Creation

Spring creates an object for every discovered Bean.

Example

```java
@Service
public class StudentService {

}
```

Spring internally performs something similar to:

```java
StudentService service =
        new StudentService();
```

The difference is that Spring manages the object's lifecycle.

---

# Step 3 — Dependency Injection

After Bean creation,

Spring injects required dependencies.

Example

```java
@RequiredArgsConstructor
public class StudentController {

    private final StudentService service;

}
```

Spring automatically provides an instance of `StudentService`.

No manual object creation is required.

---

# Step 4 — @PostConstruct

After dependency injection,

Spring calls methods annotated with

```java
@PostConstruct
```

Example

```java
@PostConstruct
public void initialize(){

    System.out.println("Bean Initialized");

}
```

Typical uses:

- Load configuration
- Initialize caches
- Open resources
- Perform startup logic

This method executes only once.

---

# Step 5 — Bean Ready

After initialization,

the Bean is ready for use.

Example

```
HTTP Request

↓

Controller

↓

StudentService Bean
```

The same Bean instance can now serve requests.

---

# Step 6 — Business Logic

The Bean performs its normal responsibilities.

Example

```java
public StudentDTO findById(Long id){

    return mapper.toDTO(
            repository.findById(id)
                    .orElseThrow(...)
    );

}
```

---

# Step 7 — @PreDestroy

When the application shuts down,

Spring calls methods annotated with

```java
@PreDestroy
```

Example

```java
@PreDestroy
public void destroy(){

    System.out.println("Bean Destroyed");

}
```

Typical uses:

- Close files
- Release resources
- Close connections
- Cleanup

---

# Bean Lifecycle Diagram

```
Application Starts

↓

Component Scan

↓

Bean Created

↓

Dependencies Injected

↓

@PostConstruct

↓

Bean Ready

↓

Application Running

↓

@PreDestroy

↓

Application Stops
```

---

# Bean Scopes

A Bean scope defines how many instances Spring creates.

Common scopes:

- Singleton
- Prototype
- Request
- Session
- Application

---

# Singleton Scope

Singleton is the default scope.

```
Application

↓

One Bean Instance

↓

All Requests
```

Example

```java
@Service
public class StudentService {

}
```

Only one instance exists during the application's lifetime.

---

# Prototype Scope

A new Bean instance is created every time it is requested.

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

---

# Singleton vs Prototype

| Singleton | Prototype |
|------------|-----------|
| Default scope | Explicit scope |
| One instance | New instance each request |
| Memory efficient | Higher object creation |
| Shared Bean | Independent Beans |

---

# Bean Lifecycle in This Repository

Example

```java
@Service
public class BeanLifecycleService {

    public BeanLifecycleService(){

        System.out.println("Constructor");

    }

    @PostConstruct
    public void initialize(){

        System.out.println("Initialized");

    }

    @PreDestroy
    public void destroy(){

        System.out.println("Destroyed");

    }

}
```

Console Output

```
Constructor

↓

@PostConstruct

↓

Application Running

↓

@PreDestroy
```

---

# Real-world Example

A cache service:

```
Application Starts

↓

Cache Bean Created

↓

@PostConstruct

↓

Load Cache

↓

Application Running

↓

@PreDestroy

↓

Clear Cache
```

---

# Best Practices

- Use Singleton scope unless another scope is required.
- Keep `@PostConstruct` lightweight.
- Release resources in `@PreDestroy`.
- Prefer constructor injection.
- Avoid expensive operations inside constructors.

---

# Common Mistakes

❌ Calling `@PostConstruct` manually.

Correct:

Spring invokes it automatically.

---

❌ Expecting `@PreDestroy` for Prototype Beans.

Correct:

Spring does not manage the full lifecycle of Prototype Beans after creation.

---

❌ Creating Beans manually using `new`.

Correct:

Let Spring create and manage Beans.

---

❌ Using field injection everywhere.

Correct:

Prefer constructor injection.

---

# Summary

The Spring IoC Container manages the complete lifecycle of every Bean.

Beans are discovered during component scanning, instantiated, injected with dependencies, initialized using `@PostConstruct`, used throughout the application's lifetime and cleaned up using `@PreDestroy`.

Understanding this lifecycle is essential for writing efficient and maintainable Spring Boot applications.

---

# Key Takeaways

- Beans are managed by the Spring IoC Container.
- Component Scanning discovers Beans.
- Spring performs Dependency Injection automatically.
- `@PostConstruct` executes after Bean initialization.
- `@PreDestroy` executes before Bean destruction.
- Singleton is the default Bean scope.
- Prototype creates a new Bean instance each time.

---

# Interview Questions

### What is a Spring Bean?

A Spring Bean is an object created and managed by the Spring IoC Container.

---

### What is the Bean Lifecycle?

The Bean Lifecycle consists of creation, dependency injection, initialization, usage and destruction managed by Spring.

---

### When is `@PostConstruct` executed?

After dependency injection and before the Bean is available for use.

---

### When is `@PreDestroy` executed?

Before the Bean is destroyed when the application shuts down.

---

### What is the default Bean scope?

Singleton.

Only one instance of the Bean exists for the entire application.

---

### Difference between Singleton and Prototype?

- Singleton: One shared Bean instance.
- Prototype: A new Bean instance is created every time it is requested.

---

# Next Chapter

**12-ioc-container.md**