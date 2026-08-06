# JVM Memory Errors

## Learning Objectives

After completing this chapter, you should be able to:

- Explain StackOverflowError.
- Explain OutOfMemoryError.
- Differentiate between StackOverflowError and OutOfMemoryError.
- Explain Memory Leaks.
- Identify common causes of JVM memory errors.
- Explain prevention techniques.
- Explain memory issues in real-world backend applications.

---

# Introduction

During program execution, the JVM manages memory automatically.

However, incorrect programming practices or excessive memory usage can still lead to runtime memory errors.

The most common JVM memory-related errors are:

- StackOverflowError
- OutOfMemoryError

Understanding these errors is essential for debugging Java backend applications.

---

# StackOverflowError

A **StackOverflowError** occurs when a thread's Stack memory is exhausted.

The most common reason is **infinite recursion** or excessive nested method calls.

---

## How It Happens

Every method call creates a new Stack Frame.

```
main()

↓

methodA()

↓

methodB()

↓

methodC()

↓

...

↓

Stack Full

↓

StackOverflowError
```

Since Stack memory is limited, continuously creating new Stack Frames eventually exhausts the available space.

---

## Common Causes

- Infinite recursion
- Missing recursion base condition
- Excessive nested method calls
- Very large stack frames

---

## Example

```java
public static void recursiveMethod() {

    recursiveMethod();

}
```

Each invocation creates another Stack Frame.

Eventually,

```
Stack

↓

Full

↓

StackOverflowError
```

---

## Prevention

- Always define a recursion base condition.
- Avoid unnecessarily deep recursion.
- Use iterative solutions when appropriate.
- Reduce excessive method nesting.

---

# OutOfMemoryError

An **OutOfMemoryError** occurs when the JVM cannot allocate additional memory because the required memory area has been exhausted.

The most common type is:

```
Java Heap Space
```

---

## How It Happens

Objects continue to be created.

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

↓

OutOfMemoryError
```

The Garbage Collector attempts to reclaim memory.

If insufficient memory is recovered,

the JVM throws

```
OutOfMemoryError
```

---

## Common Types

- Java Heap Space
- GC Overhead Limit Exceeded
- Metaspace
- Direct Buffer Memory
- Unable to Create Native Thread

For backend interviews,

the most important type is

```
Java Heap Space
```

---

## Example

```java
List<byte[]> list = new ArrayList<>();

while (true) {

    list.add(new byte[1024 * 1024]);

}
```

The Heap continues to grow until memory is exhausted.

---

## Prevention

- Release unnecessary object references.
- Avoid excessive object creation.
- Use efficient collections.
- Configure appropriate Heap size.
- Monitor memory usage.

---

# Memory Leak

A Memory Leak occurs when objects are no longer needed but remain reachable.

Since they are still reachable,

the Garbage Collector cannot reclaim them.

---

## Example

```java
private static final List<Object> CACHE = new ArrayList<>();
```

```
Request

↓

Create Object

↓

Store in Static List

↓

Never Remove

↓

Heap Grows Forever
```

Although the objects are unused,

they remain reachable through the static collection.

---

## Common Causes

- Static collections
- Uncleared caches
- Long-lived object references
- Unclosed resources
- Event listener registrations

---

## Prevention

- Remove unused objects from collections.
- Clear caches when appropriate.
- Close files, sockets and database connections.
- Use bounded caches.
- Avoid unnecessary static state.

---

# StackOverflowError vs OutOfMemoryError

| StackOverflowError | OutOfMemoryError |
|--------------------|------------------|
| Stack memory exhausted | Heap or another memory area exhausted |
| Caused by excessive method calls | Caused by excessive memory allocation |
| Usually recursion | Usually object creation |
| Thread-specific | Commonly affects shared Heap |
| Stack Frames | Objects and memory allocations |

---

# Memory Lifecycle

```
Program Starts

↓

Objects Created

↓

Heap Allocation

↓

Method Calls

↓

Stack Frames

↓

Objects Become Unreachable

↓

Garbage Collection

↓

Memory Reused
```

If objects remain reachable unnecessarily,

```
Memory Leak

↓

Heap Growth

↓

OutOfMemoryError
```

If recursion never terminates,

```
Stack Frames

↓

Stack Full

↓

StackOverflowError
```

---

# Real-world Examples

## StackOverflowError

Recursive JSON parsing without a termination condition.

Recursive tree traversal with cyclic data.

Infinite recursive method calls.

---

## OutOfMemoryError

Loading extremely large files into memory.

Reading millions of database records without pagination.

Creating excessively large caches.

Large image processing.

---

## Memory Leak

Growing static collections.

Session objects never removed.

Application-level caches without eviction.

Long-lived listeners.

---

# Common Misconceptions

❌ StackOverflowError is caused by Heap memory.

Correct:

It occurs because Stack memory is exhausted.

---

❌ OutOfMemoryError only occurs because of Memory Leaks.

Correct:

It can also occur when legitimate memory requirements exceed available memory.

---

❌ Garbage Collector prevents every Memory Leak.

Correct:

GC only removes unreachable objects.

Reachable objects remain in memory.

---

# Summary

StackOverflowError occurs when Stack memory is exhausted.

OutOfMemoryError occurs when the JVM cannot allocate additional memory.

Memory Leaks occur when unnecessary objects remain reachable.

Understanding these concepts is essential for building scalable backend applications.

---

# Key Takeaways

- StackOverflowError affects Stack memory.
- OutOfMemoryError commonly affects Heap memory.
- Memory Leaks keep objects reachable.
- Garbage Collector only removes unreachable objects.
- Proper memory management improves application performance and stability.

---

# Next Chapter

**10-interview-handbook.md**