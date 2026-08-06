# Servlet Filters

## Learning Objectives

After completing this chapter, you should be able to:

- Explain what a Servlet Filter is.
- Explain why Filters exist.
- Explain where Filters execute.
- Explain the Filter lifecycle.
- Understand the FilterChain.
- Differentiate Filters from Interceptors.
- Identify real-world Filter use cases.

---

# Introduction

A Servlet Filter is a component provided by the Java Servlet API.

It processes every incoming HTTP request before the request reaches Spring MVC.

Similarly, it can process every outgoing HTTP response before it is returned to the client.

Filters are **not** part of Spring MVC.

They belong to the Servlet Container (Tomcat).

---

# Where does a Filter execute?

A Filter executes immediately after the Servlet Container receives the request.

```
HTTP Client

↓

Embedded Tomcat

↓

Servlet Filter

↓

DispatcherServlet

↓

Controller
```

For the response:

```
Controller

↓

DispatcherServlet

↓

Servlet Filter

↓

HTTP Client
```

---

# Why do we need Filters?

Filters provide common functionality that should execute for every request.

Typical examples include:

- Logging
- Authentication
- CORS
- Request Timing
- Compression
- Encoding
- Request/Response Modification

Instead of repeating the same code in every controller, a Filter centralizes this behavior.

---

# Filter Lifecycle

A Filter has three important lifecycle methods.

```
init()

↓

doFilter()

↓

destroy()
```

---

# init()

Called once when the Filter is initialized.

Used for:

- Reading configuration
- Initializing resources

---

# doFilter()

The most important method.

Every HTTP request passes through this method.

Responsibilities:

- Inspect request
- Modify request
- Continue request processing
- Inspect response
- Modify response

Example

```java
chain.doFilter(request, response);
```

This forwards the request to the next Filter or to Spring MVC.

---

# destroy()

Called once when the application shuts down.

Used for:

- Cleanup
- Closing resources

---

# FilterChain

Multiple Filters can be chained together.

```
Request

↓

Filter A

↓

Filter B

↓

Filter C

↓

DispatcherServlet
```

Each Filter decides whether to continue processing.

---

# LoggingFilter Example

In this repository

```java
@Component
public class LoggingFilter implements Filter {

    @Override
    public void doFilter(
            ServletRequest request,
            ServletResponse response,
            FilterChain chain)
            throws IOException, ServletException {

        System.out.println("Before Request");

        chain.doFilter(request, response);

        System.out.println("After Response");
    }

}
```

Execution

```
Request

↓

Before Request

↓

Controller

↓

After Response
```

---

# Request Flow

```
HTTP Request

↓

Tomcat

↓

LoggingFilter

↓

DispatcherServlet

↓

Controller
```

---

# Response Flow

```
Controller

↓

DispatcherServlet

↓

LoggingFilter

↓

HTTP Response
```

---

# Real-world Use Cases

## Logging

Measure request execution time.

---

## Authentication

Verify authentication tokens before the request enters Spring MVC.

---

## CORS

Add Cross-Origin Resource Sharing headers.

---

## Compression

Compress HTTP responses.

---

## Character Encoding

Configure UTF-8 encoding.

---

## Security

Reject malicious requests before reaching controllers.

---

# Advantages

- Centralized request processing
- Executes before Spring MVC
- Reusable
- Independent of controllers
- Suitable for cross-cutting concerns

---

# Limitations

Filters do not know:

- Which controller will execute
- Which service will execute
- Which handler method is selected

Only DispatcherServlet has this information.

---

# Filter Execution Order

```
HTTP Request

↓

Tomcat

↓

Filter

↓

DispatcherServlet

↓

HandlerMapping

↓

Controller
```

---

# Common Mistakes

❌ Filters are part of Spring MVC.

Correct:

Filters belong to the Servlet API.

---

❌ Filters execute after DispatcherServlet.

Correct:

Filters execute before DispatcherServlet.

---

❌ Filters know which controller executes.

Correct:

Filters execute before controller resolution.

---

# Summary

Filters are Servlet components that process HTTP requests before they enter Spring MVC and process responses before they leave the application.

They are ideal for cross-cutting concerns such as logging, authentication, CORS and request timing.

---

# Key Takeaways

- Filters belong to the Servlet Container.
- Filters execute before DispatcherServlet.
- Every request passes through registered Filters.
- `doFilter()` is the primary processing method.
- Filters are ideal for logging and security.

---

# Interview Questions

### What is a Servlet Filter?

A Servlet Filter is a Servlet API component that intercepts HTTP requests and responses before they enter or leave Spring MVC.

---

### Why do we use Filters?

To implement cross-cutting concerns such as logging, authentication, CORS and request timing.

---

### Which method forwards the request?

```java
chain.doFilter(request, response);
```

---

### Can multiple Filters exist?

Yes.

They execute through a FilterChain.

---

### Does a Filter know which controller executes?

No.

Controller resolution happens later inside DispatcherServlet.

---

# Next Chapter

**06-interceptor.md**