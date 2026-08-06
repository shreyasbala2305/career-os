# Garbage Collection

## Learning Objectives

After completing this chapter, you should be able to:

- Explain Garbage Collection (GC).
- Explain why Garbage Collection is required.
- Explain object eligibility.
- Explain Reachability Analysis.
- Explain GC Roots.
- Explain Young and Old Generation.
- Explain Minor, Major and Full GC.
- Compare Serial GC and G1 GC.
- Explain `System.gc()`.
- Explain why `finalize()` is deprecated.
- Explain Garbage Collection in real-world backend applications.

---

# Introduction

Java automatically manages memory using the **Garbage Collector (GC)**.

Unlike languages such as C and C++, developers do not manually free memory.

Instead, the JVM automatically identifies objects that are no longer reachable and reclaims their memory.

This reduces memory leaks, improves application stability and simplifies development.

---

# Why Garbage Collection Exists

Suppose a Java application continuously creates objects.

```
Object

↓

Object

↓

Object

↓

Object

↓

Heap Full
```

Without Garbage Collection,

the application would eventually run out of memory.

Garbage Collection automatically removes objects that are no longer needed, allowing the Heap to be reused.

---

# How Garbage Collection Works

The Garbage Collector **does not remove objects simply because they are old**.

Instead, it removes objects that are **no longer reachable**.

```
Reachable Object

↓

Remain in Heap

----------------------

Unreachable Object

↓

Eligible for Garbage Collection
```

---

# Object Eligibility

An object becomes **eligible for Garbage Collection** when no active reference points to it.

Example

```java
Employee employee = new Employee();

employee = null;
```

After

```java
employee = null;
```

the object has no reachable reference and becomes eligible for Garbage Collection.

Important:

> **Eligible** does not mean **immediately collected**.

The JVM decides when to run GC.

---

# Reachability Analysis

Modern JVMs use **Reachability Analysis** to determine whether an object is alive.

The JVM starts from a set of special references called **GC Roots**.

If an object can be reached from any GC Root, it remains alive.

Otherwise,

the object becomes eligible for Garbage Collection.

```
GC Root

↓

Object A

↓

Object B

↓

Object C
```

All objects remain reachable.

---

If

```
GC Root

↓

Object A
```

and

```
Object B

↓

Object C
```

are disconnected,

then

```
Object B

Object C
```

become eligible for Garbage Collection.

---

# GC Roots

GC Roots are special references that always remain alive.

Examples include:

- Local variables in Stack Frames
- Active Threads
- Static fields
- JNI References

The Garbage Collector begins its search from these roots.

---

# Heap Generations

The JVM divides the Heap into generations to improve GC performance.

```
Heap

│

├── Young Generation

└── Old Generation
```

---

# Young Generation

New objects are allocated in the Young Generation.

Most objects have a very short lifetime.

Examples

- Request objects
- Temporary Strings
- DTOs
- Local collections

Garbage Collection here is called

```
Minor GC
```

Minor GC is generally fast.

---

# Old Generation

Objects that survive multiple Minor GCs are promoted to the Old Generation.

Examples

- Cached objects
- Singleton objects
- Long-lived services

Garbage Collection here is more expensive.

---

# Minor GC

Minor GC cleans the Young Generation.

Characteristics

- Fast
- Frequent
- Low pause time

---

# Major GC

Major GC primarily targets the Old Generation.

Characteristics

- Slower
- Less frequent
- Longer pause times

---

# Full GC

Full GC performs garbage collection across the entire Heap.

It is the most expensive type of Garbage Collection.

Applications may experience noticeable pause times during Full GC.

---

# System.gc()

The method

```java
System.gc();
```

does **not** force Garbage Collection.

It merely requests that the JVM perform GC.

The JVM may:

- Run Garbage Collection
- Delay it
- Ignore the request

Therefore,

developers should not rely on

```java
System.gc();
```

for application logic.

---

# finalize()

Older versions of Java allowed objects to override

```java
protected void finalize()
```

before collection.

However,

`finalize()` is:

- Unpredictable
- Slow
- Difficult to optimize

It has been **deprecated** and should not be used in modern Java applications.

Use

- `try-with-resources`
- `AutoCloseable`

for resource management instead.

---

# Serial GC

Serial GC uses a **single thread** for Garbage Collection.

Characteristics

- Simple
- Low overhead
- Suitable for small applications
- Stop-the-world collection

---

# G1 Garbage Collector

G1 (Garbage First) is the default collector in modern JVMs.

Characteristics

- Divides Heap into regions
- Prioritizes regions with the most reclaimable memory
- Better pause-time predictability
- Suitable for large backend applications

---

# Serial GC vs G1 GC

| Serial GC | G1 GC |
|------------|--------|
| Single-threaded | Multi-threaded |
| Best for small applications | Best for large applications |
| Longer pauses | Lower pause times |
| Simpler implementation | Region-based collection |

---

# Real-world Example

Suppose a Spring Boot application receives thousands of HTTP requests.

For every request,

the JVM creates

- Controller objects
- DTOs
- JSON objects
- Temporary Strings

After the request completes,

these objects become unreachable.

The Garbage Collector reclaims their memory.

Meanwhile,

Singleton Beans remain reachable and are **not** collected because Spring continues to reference them.

---

# Common Misconceptions

❌ Garbage Collector removes every unused object immediately.

Correct:

Objects are collected only when the JVM decides to perform GC.

---

❌ Calling `System.gc()` forces Garbage Collection.

Correct:

It only requests GC.

---

❌ Objects are collected because they become old.

Correct:

Objects are collected because they become unreachable.

---

❌ Java applications cannot have memory leaks.

Correct:

Java can still have memory leaks if objects remain reachable.

---

# Summary

Garbage Collection is responsible for reclaiming memory occupied by unreachable objects.

Modern JVMs determine object eligibility using Reachability Analysis and GC Roots.

The Heap is divided into Young and Old Generations to improve performance.

G1 is the default Garbage Collector in modern JVMs because it provides predictable pause times and scales well for backend applications.

---

# Key Takeaways

- Garbage Collection automatically manages Heap memory.
- Objects become eligible when they are unreachable.
- Reachability Analysis starts from GC Roots.
- Minor GC cleans the Young Generation.
- Major GC targets the Old Generation.
- Full GC scans the entire Heap.
- `System.gc()` is only a request.
- `finalize()` is deprecated.
- G1 is the default collector in modern JVMs.

---

# Next Chapter

**07-reference-types.md**