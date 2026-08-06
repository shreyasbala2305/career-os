# Spring Boot Architecture

## Learning Objectives

After completing this chapter, you should be able to:

- Explain Spring Boot Architecture.
- Explain ApplicationContext.
- Explain IoC Container.
- Explain BeanFactory.
- Explain Component Scanning.
- Explain Auto Configuration.
- Explain Spring Boot startup process.
- Understand how Spring Boot initializes an application.

---

# Introduction

Spring Boot applications consist of several core components working together to create and manage the application.

Unlike traditional Java applications, Spring Boot automatically creates and configures most infrastructure components.

The startup process begins with:

```java
SpringApplication.run(
    SpringbootRequestLifecycleApplication.class,
    args
);
```

This single statement starts the complete Spring Boot application.

---

# Spring Boot Startup Flow

```
main()

↓

SpringApplication.run()

↓

Create ApplicationContext

↓

Component Scanning

↓

Bean Creation

↓

Dependency Injection

↓

@PostConstruct

↓

Embedded Tomcat Starts

↓

Application Ready
```

---

# Main Components

A typical Spring Boot application consists of:

- SpringApplication
- ApplicationContext
- IoC Container
- BeanFactory
- Component Scanner
- Auto Configuration
- Embedded Tomcat

---

# SpringApplication

`SpringApplication` is responsible for bootstrapping the application.

Responsibilities include:

- Starting the application
- Creating the ApplicationContext
- Loading configuration
- Performing component scanning
- Starting the embedded server

Example

```java
@SpringBootApplication
public class SpringbootRequestLifecycleApplication {

    public static void main(String[] args) {

        SpringApplication.run(
                SpringbootRequestLifecycleApplication.class,
                args
        );

    }

}
```

---

# ApplicationContext

ApplicationContext is the central container in Spring Boot.

It manages:

- Bean creation
- Bean lifecycle
- Dependency Injection
- Configuration
- Event publishing

Think of it as the "brain" of a Spring application.

---

# IoC Container

IoC (Inversion of Control) means that Spring manages object creation instead of the developer.

Without IoC:

```java
StudentService service =
        new StudentService();
```

With IoC:

```java
private final StudentService service;
```

Spring creates and injects the object automatically.

---

# BeanFactory

BeanFactory is the basic Spring container.

Responsibilities:

- Create beans
- Store beans
- Return beans when requested

ApplicationContext extends BeanFactory and provides many additional enterprise features.

---

# Component Scanning

Spring scans the package containing the main application class and its sub-packages.

Classes annotated with:

- `@Component`
- `@Service`
- `@Repository`
- `@Controller`
- `@RestController`
- `@Configuration`

are automatically detected and registered as Spring Beans.

---

# Bean Creation

After component scanning,

Spring creates bean instances.

Example:

```
StudentController

↓

StudentService

↓

StudentRepository
```

Each becomes a managed Spring Bean.

---

# Dependency Injection

Once beans are created,

Spring resolves dependencies.

Example:

```
StudentController

↓

StudentService

↓

StudentRepository
```

Spring injects these dependencies automatically, usually through constructor injection.

---

# Auto Configuration

Spring Boot automatically configures many components based on the dependencies present in the project.

Example:

Adding

```xml
spring-boot-starter-web
```

automatically configures:

- DispatcherServlet
- Jackson
- Embedded Tomcat
- Spring MVC
- Error handling

No XML configuration is required.

---

# Embedded Tomcat

Spring Boot packages an embedded servlet container inside the application.

Startup sequence:

```
ApplicationContext Ready

↓

Embedded Tomcat Starts

↓

Port 8080 Opens

↓

Application Ready
```

The application can now receive HTTP requests.

---

# Startup Sequence

```
Application Starts

↓

SpringApplication

↓

ApplicationContext

↓

Component Scan

↓

Bean Creation

↓

Dependency Injection

↓

@PostConstruct

↓

Embedded Tomcat

↓

Ready to Accept Requests
```

---

# Architecture Diagram

```
SpringApplication.run()

↓

ApplicationContext

↓

IoC Container

↓

BeanFactory

↓

Component Scanner

↓

Bean Creation

↓

Dependency Injection

↓

Embedded Tomcat

↓

HTTP Requests
```

---

# Real-world Example

In this repository:

```
StudentController

↓

StudentService

↓

StudentRepository
```

These classes are automatically:

- Discovered
- Instantiated
- Wired together

No manual object creation is required.

---

# Common Misconceptions

❌ ApplicationContext and BeanFactory are the same.

Correct:

BeanFactory is the basic container.

ApplicationContext builds upon BeanFactory and provides additional enterprise features.

---

❌ Spring creates objects only when they are first used.

Correct:

Singleton beans are generally created during application startup.

---

❌ Auto Configuration replaces Spring Framework.

Correct:

Auto Configuration simplifies Spring configuration; it does not replace the framework.

---

# Summary

Spring Boot starts by creating the ApplicationContext, scanning components, creating beans, injecting dependencies, and starting the embedded Tomcat server.

These steps prepare the application to handle incoming HTTP requests.

---

# Key Takeaways

- `SpringApplication.run()` starts the application.
- ApplicationContext is the central Spring container.
- IoC manages object creation.
- Component Scanning discovers beans automatically.
- Dependency Injection connects application components.
- Embedded Tomcat allows applications to run as standalone JARs.
- Auto Configuration minimizes manual configuration.

---

# Next Chapter

**03-dispatcherservlet.md**