# Pass By Value

## Learning Objectives

After completing this chapter, you should be able to:

- Explain Java's parameter passing mechanism.
- Explain why Java is always pass-by-value.
- Differentiate between primitive values and object references.
- Explain why object state can change after a method call.
- Explain why reassigning a reference inside a method does not affect the caller.
- Answer common interview questions related to Java parameter passing.

---

# Introduction

One of the most misunderstood concepts in Java is parameter passing.

Many developers believe Java supports **pass-by-reference**.

This is incorrect.

Java is **always pass-by-value**.

The confusion occurs because object references are passed by value.

---

# What is Pass By Value?

Pass-by-value means the called method receives **a copy of the argument**.

Changes made to that copy do not affect the original variable.

```
Original Value

↓

Copy Created

↓

Method Receives Copy
```

The original variable remains unchanged.

---

# Primitive Values

Primitive variables store actual values.

Example

```java
int number = 10;
```

When

```java
modify(number);
```

is called,

the method receives

```
10

(Copy)
```

Memory

```
main()

number = 10

↓

modify()

value = 10
```

Changing

```java
value = 100;
```

does not change

```java
number
```

because only the copied value is modified.

---

# Object References

Objects are stored in the Heap.

Variables store references to those objects.

Example

```java
Employee employee = new Employee();
```

Memory

```
Stack

employee

↓

Heap

Employee Object
```

When

```java
modify(employee);
```

is called,

Java copies the **reference value**.

```
employee

↓

Employee Object

--------------------

employeeCopy

↓

Same Employee Object
```

Both references point to the same object.

---

# Modifying Object State

Suppose

```java
employee.setName("Rahul");
```

Both references point to the same object.

Therefore,

changing the object's state is visible outside the method.

```
Caller

↓

Employee Object

↑

Method
```

The object changes,

not the reference.

---

# Reassigning the Reference

Suppose

```java
employee = new Employee();
```

inside the method.

Only the copied reference changes.

Memory

Before

```
Caller

↓

Employee A

Method

↓

Employee A
```

After reassignment

```
Caller

↓

Employee A

Method

↓

Employee B
```

The caller still points to

```
Employee A
```

Therefore,

the original reference remains unchanged.

---

# Primitive vs Object

| Primitive | Object Reference |
|------------|------------------|
| Value copied | Reference value copied |
| Original unchanged | Object state may change |
| Independent copies | Both references point to same object |

---

# Common Misconception

Many developers think

```
Object

↓

Pass By Reference
```

This is incorrect.

Java copies the **reference value**, not the object itself.

---

# Why the Confusion?

Suppose

```java
Employee employee = new Employee();
```

Both the caller and the method contain copies of the same reference.

```
Caller

↓

Employee

↑

Method
```

Changing the object's fields is visible through both references.

This often creates the misconception that Java passed the object by reference.

In reality,

only the reference value was copied.

---

# Real-world Example

Spring Boot Controller

```java
User user = service.findById(1);

updateUser(user);
```

If

```java
updateUser()
```

changes

```java
user.setName("John");
```

the caller observes the updated object.

However,

if

```java
updateUser()
```

executes

```java
user = new User();
```

the caller still references the original object.

---

# Common Misconceptions

❌ Java supports pass-by-reference.

Correct:

Java always uses pass-by-value.

---

❌ Objects are copied during method calls.

Correct:

Only the reference value is copied.

---

❌ Reassigning a parameter changes the caller's variable.

Correct:

Only the local copy of the reference changes.

---

# Summary

Java always passes arguments by value.

For primitive variables,

the value itself is copied.

For objects,

the reference value is copied.

Both references point to the same object,

which is why changes to object state are visible.

Reassigning the reference inside the method affects only the copied reference.

---

# Key Takeaways

- Java always uses pass-by-value.
- Primitive values are copied.
- Object reference values are copied.
- Objects are never passed by reference.
- Modifying object state affects the original object.
- Reassigning the reference does not affect the caller.

---

# Next Chapter

**09-memory-errors.md**