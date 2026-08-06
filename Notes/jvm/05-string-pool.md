# String Pool

## Learning Objectives

After completing this chapter, you should be able to:

- Explain what the String Pool is.
- Explain why the String Pool exists.
- Differentiate between String literals and `new String()`.
- Explain `==` vs `equals()`.
- Explain `intern()`.
- Explain compile-time and runtime string concatenation.
- Explain how Strings are stored in JVM memory.

---

# Introduction

Strings are one of the most frequently used objects in Java.

Creating duplicate String objects wastes memory.

To optimize memory usage, the JVM maintains a special memory area called the **String Pool**.

Instead of creating duplicate String objects, identical String literals share the same object.

---

# What is the String Pool?

The String Pool is a special area inside the JVM where String literals are stored.

Whenever the same String literal appears multiple times, the JVM reuses the existing String object instead of creating a new one.

Example

```java
String language1 = "Java";
String language2 = "Java";
```

Both variables refer to the same object.

---

# Why Does the String Pool Exist?

Suppose an application contains

```java
String role = "ADMIN";
```

in thousands of places.

Without the String Pool

```
ADMIN

↓

New Object

↓

New Object

↓

New Object

↓

Thousands of Objects
```

Huge memory waste.

With the String Pool

```
ADMIN

↓

One Object

↓

Shared Everywhere
```

Advantages

- Saves memory
- Improves performance
- Reduces duplicate object creation
- Faster comparison using references

---

# String Literal

When a String literal is created

```java
String language = "Java";
```

the JVM first checks the String Pool.

If

```
"Java"
```

already exists,

the existing object is reused.

Otherwise,

a new String object is added to the pool.

---

# Memory Representation

```java
String first = "Java";

String second = "Java";
```

```
Stack

first --------+

second -------+

               |

               ▼

String Pool

+-----------+

"Java"

+-----------+
```

Only one String object exists.

---

# Using new String()

Creating a String with

```java
new String("Java")
```

always creates a new object in the Heap.

Example

```java
String first = new String("Java");

String second = new String("Java");
```

Memory

```
Stack

first ----------+

                 |

                 ▼

Heap

String

                 |

                 ▼

String Pool

"Java"

-----------------------

second ---------+

                 |

                 ▼

Heap

Another String

                 |

                 ▼

String Pool

"Java"
```

Two Heap objects are created.

The pooled String still exists separately.

---

# String Literal vs new String()

| String Literal | new String() |
|---------------|--------------|
| Uses String Pool | Creates new Heap object |
| Memory Efficient | More memory usage |
| Object may be reused | New object every time |
| Preferred | Rarely required |

---

# == vs equals()

## ==

The `==` operator compares object references.

Example

```java
String first = "Java";

String second = "Java";
```

```
first == second

↓

true
```

because both references point to the same pooled object.

---

## equals()

`equals()` compares the actual character sequence.

Example

```java
String first = new String("Java");

String second = new String("Java");
```

```
first.equals(second)

↓

true
```

because both Strings contain the same value.

---

# Comparison Example

```java
String first = "Java";

String second = new String("Java");
```

```
first == second

↓

false

-----------------------

first.equals(second)

↓

true
```

---

# intern()

The `intern()` method returns the String Pool reference for a String.

Example

```java
String first = new String("Java");

String second = first.intern();
```

Memory

```
Heap

String Object

↓

intern()

↓

String Pool

"Java"
```

Now

```java
String third = "Java";
```

```
second == third

↓

true
```

because both point to the pooled String.

---

# Compile-Time String Concatenation

```java
String value = "Ja" + "va";
```

The compiler evaluates this during compilation.

Equivalent to

```java
String value = "Java";
```

The resulting String is stored in the String Pool.

---

# Runtime String Concatenation

```java
String part = "Ja";

String value = part + "va";
```

This happens at runtime.

A new String object is created.

Therefore

```java
String first = "Java";

System.out.println(first == value);
```

returns

```
false
```

---

# Memory Comparison

## Compile Time

```
"Ja"

+

"va"

↓

Compiler

↓

"Java"

↓

String Pool
```

---

## Runtime

```
Variable

+

String

↓

Runtime

↓

New Heap Object
```

---

# Real-world Example

Suppose a backend application frequently uses

```java
"ADMIN"
```

```
"USER"

```

```
"SUCCESS"
```

```
"FAILED"
```

The JVM stores each literal only once.

Every class shares the same pooled Strings.

This significantly reduces memory usage.

---

# Performance Considerations

Prefer

```java
String role = "ADMIN";
```

instead of

```java
String role = new String("ADMIN");
```

unless creating a separate object is absolutely necessary.

---

# Common Misconceptions

❌ Every String is stored in the String Pool.

Correct:

Only String literals (and Strings returned by `intern()`) are stored in the String Pool.

---

❌ `new String()` uses the pooled object.

Correct:

`new String()` always creates a new Heap object.

---

❌ `==` compares String contents.

Correct:

`==` compares references.

Use `equals()` to compare String values.

---

❌ `intern()` creates another String object.

Correct:

`intern()` returns the pooled reference if it exists or adds the String to the pool if necessary.

---

# Summary

The String Pool is a JVM optimization that stores String literals.

It avoids duplicate objects and improves memory efficiency.

Use String literals whenever possible.

Use `equals()` to compare String values.

Use `intern()` when a pooled reference is required.

---

# Key Takeaways

- String literals use the String Pool.
- `new String()` creates a new Heap object.
- `==` compares references.
- `equals()` compares values.
- `intern()` returns the pooled String.
- Compile-time concatenation uses the String Pool.
- Runtime concatenation creates a new object.

---

# Next Chapter

**06-garbage-collection.md**