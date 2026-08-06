# Reference Types

## Learning Objectives

After completing this chapter, you should be able to:

- Explain Java Reference Types.
- Explain Strong References.
- Explain Weak References.
- Explain Soft References.
- Explain Phantom References.
- Compare all four reference types.
- Understand real-world use cases.

---

# Introduction

Every object created in Java is accessed through a reference.

The type of reference determines how the Garbage Collector treats that object.

Java provides four reference types:

```
Strong Reference

↓

Soft Reference

↓

Weak Reference

↓

Phantom Reference
```

Each has different behavior during Garbage Collection.

---

# Strong Reference

A Strong Reference is the default reference type in Java.

Example

```java
Employee employee = new Employee();
```

The object remains alive as long as a strong reference exists.

```
Stack

employee

↓

Heap

Employee Object
```

Even if

```java
System.gc();
```

is called,

the object will **not** be collected while the strong reference exists.

---

## Characteristics

- Default reference type
- Prevents Garbage Collection
- Most commonly used
- Object survives GC while reachable

---

## Example

```java
Employee employee = new Employee();

System.gc();
```

The object remains alive.

---

# Weak Reference

A Weak Reference does **not** prevent Garbage Collection.

It is created using

```java
WeakReference<T>
```

Example

```java
WeakReference<Employee> reference =
        new WeakReference<>(new Employee());
```

If no strong reference exists,

the object becomes eligible for Garbage Collection.

---

## Characteristics

- Collected during the next GC cycle
- Suitable for caches
- Does not keep objects alive

---

## Example

```
Strong Reference Removed

↓

Only Weak Reference Exists

↓

Garbage Collector

↓

Object Removed
```

---

# Soft Reference

Soft References are slightly stronger than Weak References.

Objects referenced by a Soft Reference remain alive until the JVM experiences memory pressure.

Example

```java
SoftReference<Employee> reference =
        new SoftReference<>(new Employee());
```

---

## Characteristics

- Survive normal Garbage Collection
- Collected only when memory is low
- Useful for memory-sensitive caches

---

# Phantom Reference

Phantom References are the weakest reference type.

They are created using

```java
PhantomReference<T>
```

Unlike other reference types,

calling

```java
get()
```

always returns

```
null
```

They are primarily used for advanced resource cleanup.

---

## Characteristics

- Used with ReferenceQueue
- Object is already unreachable
- Mainly used by JVM frameworks and libraries

---

# Reference Strength

```
Strong

↓

Soft

↓

Weak

↓

Phantom
```

As the reference strength decreases,

the likelihood of Garbage Collection increases.

---

# Garbage Collection Behavior

| Reference Type | Eligible for GC? |
|----------------|------------------|
| Strong | ❌ No |
| Soft | Only under memory pressure |
| Weak | Yes |
| Phantom | Yes |

---

# Reference Comparison

| Feature | Strong | Soft | Weak | Phantom |
|---------|---------|------|------|----------|
| Prevents GC | ✅ | Mostly | ❌ | ❌ |
| Memory Sensitive | ❌ | ✅ | ❌ | ❌ |
| Cache Usage | ❌ | ✅ | Limited | ❌ |
| get() Returns Object | ✅ | ✅ | ✅ (until collected) | ❌ |

---

# Real-world Examples

## Strong Reference

Application Services

```
Controller

↓

Service

↓

Repository
```

These objects should remain alive.

---

## Soft Reference

Image Cache

```
Image

↓

Soft Reference

↓

Removed only when memory is low
```

---

## Weak Reference

Metadata Cache

```
Cached Object

↓

Weak Reference

↓

Automatically removed during GC
```

---

## Phantom Reference

JVM internals

Native resource cleanup

Memory management libraries

---

# Common Misconceptions

❌ Weak References never survive Garbage Collection.

Correct:

They may survive until a GC cycle actually runs.

---

❌ Soft References are never collected.

Correct:

They are collected when the JVM needs memory.

---

❌ Phantom References provide object access.

Correct:

`get()` always returns `null`.

---

# Summary

Java provides four reference types with different Garbage Collection behavior.

Strong References keep objects alive.

Soft References are suitable for memory-sensitive caches.

Weak References allow objects to be reclaimed during Garbage Collection.

Phantom References are primarily used for advanced cleanup mechanisms.

---

# Key Takeaways

- Strong Reference is the default reference type.
- Weak References do not prevent Garbage Collection.
- Soft References survive until memory pressure increases.
- Phantom References are used with `ReferenceQueue`.
- Reference strength determines Garbage Collection eligibility.

---

# Next Chapter

**08-pass-by-value.md**