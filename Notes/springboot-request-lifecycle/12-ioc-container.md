# Spring IoC Container & Dependency Injection

## Learning Objectives

After completing this chapter, you should be able to:

- Explain Inversion of Control (IoC).
- Explain Dependency Injection (DI).
- Differentiate BeanFactory and ApplicationContext.
- Understand Component Scanning.
- Explain Bean creation inside the IoC Container.
- Compare Constructor, Setter and Field Injection.
- Understand Circular Dependencies (basic).
- Answer common IoC interview questions.

---

# Introduction

The Spring IoC (Inversion of Control) Container is the heart of the Spring Framework.

Instead of developers creating and managing objects manually,

Spring creates, configures and manages application objects (Beans).

This makes applications:

- Loosely coupled
- Easier to test
- Easier to maintain
- Easier to extend

---

# What is Inversion of Control (IoC)?

Inversion of Control is a design principle where object creation and lifecycle management are delegated to a container instead of being handled by application code.

Without IoC

```java
StudentRepository repository = new StudentRepository();

StudentService service =
        new StudentService(repository);
```

The developer is responsible for creating every dependency.

With IoC

```java
@Service
public class StudentService {

    private final StudentRepository repository;

}
```

Spring creates the objects and injects the dependencies automatically.

---

# Why is it called "Inversion" of Control?

Normally,

the application controls object creation.

```
Developer

↓

new Object()
```

With Spring,

the container controls object creation.

```
Developer

↓

Spring Container

↓

Bean
```

Control has been inverted from the developer to the framework.

---

# What is Dependency Injection (DI)?

Dependency Injection is the process of providing an object's dependencies instead of allowing the object to create them.

Example

Without DI

```java
public class StudentService {

    private StudentRepository repository =
            new StudentRepository();

}
```

With DI

```java
@Service
@RequiredArgsConstructor
public class StudentService {

    private final StudentRepository repository;

}
```

Spring injects the repository automatically.

---

# IoC Container Workflow

```
Application Starts

↓

ApplicationContext Created

↓

Component Scan

↓

Bean Creation

↓

Dependency Injection

↓

@PostConstruct

↓

Application Ready
```

---

# BeanFactory

BeanFactory is the simplest IoC container.

Responsibilities:

- Create Beans
- Store Beans
- Return Beans

It provides only basic dependency management.

---

# ApplicationContext

ApplicationContext extends BeanFactory.

Additional features include:

- Event Publishing
- Internationalization (i18n)
- Resource Loading
- Annotation Support
- Bean Lifecycle Management
- Environment Support

Most Spring Boot applications use ApplicationContext.

---

# BeanFactory vs ApplicationContext

| BeanFactory | ApplicationContext |
|-------------|--------------------|
| Basic IoC Container | Advanced IoC Container |
| Lazy Bean Loading | Eager Singleton Bean Creation |
| Limited Features | Enterprise Features |
| Rarely used directly | Used by Spring Boot |

---

# Component Scanning

Spring automatically scans packages for components.

Common stereotypes:

```java
@Component

@Service

@Repository

@Controller

@RestController

@Configuration
```

Each discovered class becomes a Spring Bean.

---

# Bean Creation Process

```
Component Found

↓

Bean Definition Created

↓

Object Instantiated

↓

Dependencies Injected

↓

@PostConstruct

↓

Bean Ready
```

---

# Dependency Injection Types

Spring supports three types of Dependency Injection.

---

## Constructor Injection

Recommended approach.

```java
@Service
@RequiredArgsConstructor
public class StudentService {

    private final StudentRepository repository;

}
```

Advantages

- Immutable dependencies
- Easier testing
- Prevents partially initialized objects

---

## Setter Injection

```java
private StudentRepository repository;

@Autowired
public void setRepository(
        StudentRepository repository){

    this.repository = repository;

}
```

Useful for optional dependencies.

---

## Field Injection

```java
@Autowired
private StudentRepository repository;
```

Simple but generally discouraged.

Reasons:

- Difficult to test
- Hidden dependencies
- Not immutable

---

# Constructor vs Setter vs Field Injection

| Constructor | Setter | Field |
|--------------|---------|-------|
| Recommended | Optional dependencies | Not recommended |
| Immutable | Mutable | Mutable |
| Easy Testing | Moderate | Difficult |
| Compile-time safety | Runtime safety | Runtime safety |

---

# Circular Dependency (Basic)

Example

```
StudentService

↓

NotificationService

↓

StudentService
```

Each Bean depends on the other.

This creates a circular dependency.

Constructor Injection exposes such problems early.

Designing services to avoid circular dependencies is generally the best approach.

---

# Real-world Example

Application Startup

```
SpringApplication.run()

↓

ApplicationContext

↓

Component Scan

↓

StudentController Bean

↓

StudentService Bean

↓

StudentRepository Bean

↓

Dependencies Injected

↓

Application Ready
```

---

# Best Practices

- Prefer Constructor Injection.
- Avoid Field Injection.
- Keep dependencies explicit.
- Design services to avoid circular dependencies.
- Let Spring manage Bean creation.

---

# Common Mistakes

❌ Creating Beans manually using `new`.

Correct:

Let Spring create and inject Beans.

---

❌ Using Field Injection everywhere.

Correct:

Prefer Constructor Injection.

---

❌ Confusing IoC with Dependency Injection.

Correct:

IoC is the design principle.

Dependency Injection is one implementation of IoC.

---

❌ Treating BeanFactory and ApplicationContext as identical.

Correct:

ApplicationContext extends BeanFactory with many enterprise features.

---

# Summary

The Spring IoC Container is responsible for creating, configuring and managing Beans.

Dependency Injection allows Spring to provide required dependencies automatically, resulting in loosely coupled and maintainable applications.

ApplicationContext is the primary IoC container used in Spring Boot and provides advanced features beyond BeanFactory.

---

# Key Takeaways

- IoC delegates object creation to Spring.
- Dependency Injection supplies required dependencies.
- ApplicationContext is the primary IoC container.
- BeanFactory provides basic Bean management.
- Constructor Injection is the recommended approach.
- Component Scanning automatically discovers Spring Beans.

---

# Interview Questions

### What is IoC?

IoC (Inversion of Control) is a design principle where object creation and lifecycle management are handled by the Spring Container instead of application code.

---

### What is Dependency Injection?

Dependency Injection is the process of supplying an object's dependencies from the Spring Container rather than creating them manually.

---

### Difference between IoC and DI?

- IoC is the design principle.
- Dependency Injection is a technique used by Spring to implement IoC.

---

### Difference between BeanFactory and ApplicationContext?

- BeanFactory provides basic Bean management.
- ApplicationContext extends BeanFactory with enterprise features such as event publishing, resource loading and lifecycle management.

---

### Which Dependency Injection type is recommended?

Constructor Injection because it promotes immutability, explicit dependencies and easier testing.

---

### Why should Field Injection be avoided?

Field Injection makes dependencies less visible, complicates testing and prevents immutable objects.

---

# Next Chapter

**13-exception-handling.md**