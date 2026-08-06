# Spring Framework & Spring Boot Overview

## Learning Objectives

After completing this chapter, you should be able to:

- Explain Spring Framework.
- Explain Spring Boot.
- Differentiate Spring Framework and Spring Boot.
- Explain why Spring Boot exists.
- Explain Embedded Tomcat.
- Explain Auto Configuration.
- Explain Starter Dependencies.
- Explain Spring Boot Architecture.

---

# Introduction

Spring is one of the most widely used Java frameworks for building enterprise applications.

Spring Boot is an extension of the Spring Framework that simplifies application development by providing auto-configuration, starter dependencies and embedded servers.

Today, most modern Java backend applications are built using Spring Boot.

---

# What is Spring Framework?

Spring Framework is an open-source Java framework that provides comprehensive infrastructure support for building enterprise applications.

Its core features include:

- Inversion of Control (IoC)
- Dependency Injection (DI)
- Spring MVC
- Data Access
- Transaction Management
- Security Integration
- AOP (Aspect-Oriented Programming)

---

# Why was Spring Framework created?

Before Spring, enterprise Java development relied heavily on complex XML configuration and tightly coupled code.

Spring introduced:

- Loose coupling
- Dependency Injection
- Simplified configuration
- Testable applications
- Modular architecture

---

# What is Spring Boot?

Spring Boot is built on top of the Spring Framework.

It reduces boilerplate configuration and allows developers to build production-ready applications quickly.

Spring Boot provides:

- Auto Configuration
- Embedded Web Server
- Starter Dependencies
- Production-ready features
- Opinionated defaults

---

# Spring Framework vs Spring Boot

| Spring Framework | Spring Boot |
|------------------|-------------|
| Requires manual configuration | Auto Configuration |
| External server required | Embedded Tomcat |
| XML configuration common | Annotation-based |
| More setup | Minimal setup |
| Flexible | Convention over configuration |

---

# Why Spring Boot?

Spring Boot was introduced to simplify enterprise application development.

It eliminates repetitive configuration, reduces development time and allows developers to focus on business logic.

---

# Embedded Tomcat

Traditional Java web applications required deployment to an external server.

```
Application

↓

WAR File

↓

External Tomcat
```

Spring Boot embeds Tomcat inside the application.

```
Spring Boot Application

↓

Embedded Tomcat

↓

Run as JAR
```

Advantages:

- Simple deployment
- Easier development
- No external server installation
- Production-ready packaging

---

# Starter Dependencies

Starter dependencies bundle commonly used libraries.

Example:

```xml
spring-boot-starter-web
```

includes:

- Spring MVC
- Jackson
- Embedded Tomcat
- Validation support

This removes the need to manage individual dependencies manually.

---

# Auto Configuration

Spring Boot automatically configures components based on the dependencies present in the project.

For example:

Adding

```xml
spring-boot-starter-web
```

automatically configures:

- DispatcherServlet
- Jackson
- Embedded Tomcat
- Spring MVC

Developers write less configuration code.

---

# Spring Boot Architecture

```
Application

↓

SpringApplication.run()

↓

ApplicationContext

↓

Component Scan

↓

Bean Creation

↓

Embedded Tomcat

↓

Application Ready
```

---

# Advantages of Spring Boot

- Faster development
- Reduced configuration
- Embedded server
- Production-ready features
- Easy dependency management
- Excellent testing support
- Strong ecosystem

---

# Real-world Usage

Spring Boot is widely used for:

- REST APIs
- Microservices
- Enterprise Applications
- Banking Systems
- E-commerce Platforms
- Healthcare Systems
- Cloud-native Applications

---

# Common Misconceptions

❌ Spring and Spring Boot are the same.

Correct:

Spring Boot is built on top of the Spring Framework.

---

❌ Spring Boot replaces Spring.

Correct:

Spring Boot simplifies the use of Spring; it does not replace it.

---

❌ Spring Boot does not use Spring MVC.

Correct:

Spring Boot uses Spring MVC internally when building web applications.

---

# Summary

Spring Framework provides the foundation for enterprise Java development.

Spring Boot builds on top of Spring by providing auto-configuration, starter dependencies and embedded servers, making application development faster and simpler.

---

# Key Takeaways

- Spring Boot is built on Spring Framework.
- Embedded Tomcat removes the need for external servers.
- Starter dependencies simplify dependency management.
- Auto Configuration reduces manual setup.
- Spring Boot is the preferred framework for modern Java backend development.

---

# Next Chapter

**02-spring-architecture.md**