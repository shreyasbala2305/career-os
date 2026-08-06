# Spring MVC Interceptors

## Learning Objectives

After completing this chapter, you should be able to:

- Explain what a Spring MVC Interceptor is.
- Explain why Interceptors exist.
- Explain where Interceptors execute.
- Explain the Interceptor lifecycle.
- Understand `preHandle()`, `postHandle()` and `afterCompletion()`.
- Differentiate Interceptors from Filters.
- Identify real-world Interceptor use cases.

---

# Introduction

An Interceptor is a Spring MVC component that intercepts HTTP requests after they enter Spring MVC but before the controller method executes.

Unlike Filters, Interceptors are aware of Spring MVC concepts such as controllers and handler methods.

Interceptors are commonly used for:

- Authentication
- Authorization
- Logging
- Performance Monitoring
- Request Auditing

---

# Where does an Interceptor execute?

Unlike Filters,

Interceptors execute **inside Spring MVC**.

```
HTTP Client

↓

Embedded Tomcat

↓

Servlet Filter

↓

DispatcherServlet

↓

Spring MVC Interceptor

↓

Controller
```

For the response:

```
Controller

↓

Spring MVC Interceptor

↓

DispatcherServlet

↓

Servlet Filter

↓

HTTP Client
```

---

# Why do we need Interceptors?

Interceptors allow developers to execute logic before and after controller execution.

Typical use cases include:

- Authentication
- Authorization
- Logging
- Request Tracking
- Performance Measurement
- Auditing

---

# Interceptor Lifecycle

Spring MVC Interceptors provide three callback methods.

```
preHandle()

↓

Controller

↓

postHandle()

↓

Response Rendering

↓

afterCompletion()
```

---

# preHandle()

Executed before the controller method.

Return value:

```java
true
```

Continue processing.

```java
false
```

Stop processing.

Typical uses:

- Authentication
- Authorization
- Logging
- Request Validation

Example

```java
@Override
public boolean preHandle(
        HttpServletRequest request,
        HttpServletResponse response,
        Object handler) {

    return true;

}
```

---

# postHandle()

Executed after the controller method returns but before the response is written.

Typical uses:

- Add response headers
- Logging
- Modify Model (MVC applications)
- Metrics collection

Example

```java
@Override
public void postHandle(
        HttpServletRequest request,
        HttpServletResponse response,
        Object handler,
        ModelAndView modelAndView) {

}
```

---

# afterCompletion()

Executed after the entire request has completed.

Typical uses:

- Cleanup
- Resource release
- Final logging
- Performance calculation

Example

```java
@Override
public void afterCompletion(
        HttpServletRequest request,
        HttpServletResponse response,
        Object handler,
        Exception ex) {

}
```

---

# Interceptor Execution Order

```
HTTP Request

↓

Filter

↓

DispatcherServlet

↓

Interceptor (preHandle)

↓

Controller

↓

Interceptor (postHandle)

↓

Jackson

↓

Interceptor (afterCompletion)

↓

Filter

↓

HTTP Response
```

---

# RequestInterceptor Example

In this repository

```java
@Component
public class RequestInterceptor
        implements HandlerInterceptor {

    @Override
    public boolean preHandle(...) {

        System.out.println("preHandle()");

        return true;

    }

    @Override
    public void postHandle(...) {

        System.out.println("postHandle()");

    }

    @Override
    public void afterCompletion(...) {

        System.out.println("afterCompletion()");

    }

}
```

Execution

```
Request

↓

preHandle()

↓

Controller

↓

postHandle()

↓

Jackson

↓

afterCompletion()

↓

Response
```

---

# Registering an Interceptor

Interceptors are registered using `WebMvcConfigurer`.

Example

```java
@Configuration
public class WebMvcConfig
        implements WebMvcConfigurer {

    @Override
    public void addInterceptors(
            InterceptorRegistry registry) {

        registry.addInterceptor(
                new RequestInterceptor());

    }

}
```

---

# Real-world Use Cases

## Authentication

Verify whether the user is logged in.

---

## Authorization

Check user roles before allowing access.

---

## Request Logging

Log request information after Spring identifies the handler.

---

## Performance Monitoring

Measure controller execution time.

---

## Audit Logging

Track who accessed which endpoint.

---

# Filter vs Interceptor

| Filter | Interceptor |
|----------|-------------|
| Servlet API | Spring MVC |
| Executes before DispatcherServlet | Executes after DispatcherServlet |
| Knows nothing about controllers | Knows the selected controller |
| Configured by Servlet Container | Configured by Spring MVC |
| Works for every servlet request | Works only inside Spring MVC |
| Used for CORS, Encoding, Compression | Used for Authentication, Authorization, Logging |

---

# Advantages

- Access to handler information
- Access to controller metadata
- Cleaner request processing
- Easy integration with Spring MVC
- Better suited for authentication and authorization

---

# Limitations

Interceptors execute only inside Spring MVC.

They cannot intercept requests handled outside Spring MVC.

---

# Common Mistakes

❌ Interceptors execute before Filters.

Correct:

Filters execute before Interceptors.

---

❌ Interceptors belong to the Servlet API.

Correct:

Interceptors belong to Spring MVC.

---

❌ Interceptors can replace Filters.

Correct:

Filters and Interceptors solve different problems and often work together.

---

# Summary

Interceptors are Spring MVC components that execute around controller methods.

They provide hooks before controller execution, after controller execution and after request completion.

Unlike Filters, they are aware of Spring MVC and the selected handler.

---

# Key Takeaways

- Interceptors belong to Spring MVC.
- `preHandle()` executes before the controller.
- `postHandle()` executes after the controller.
- `afterCompletion()` executes after the response is completed.
- Interceptors are commonly used for authentication, authorization and logging.

---

# Interview Questions

### What is an Interceptor?

An Interceptor is a Spring MVC component that intercepts requests before and after controller execution.

---

### What is the difference between `preHandle()` and `postHandle()`?

- `preHandle()` executes before the controller.
- `postHandle()` executes after the controller but before the response is rendered.

---

### When is `afterCompletion()` executed?

After the complete request lifecycle finishes and the response has been sent.

---

### Can an Interceptor stop a request?

Yes.

Returning

```java
false
```

from `preHandle()` prevents the request from reaching the controller.

---

### Which executes first: Filter or Interceptor?

Filter.

The request enters the Servlet container before it reaches Spring MVC.

---

# Next Chapter

**07-controller-layer.md**