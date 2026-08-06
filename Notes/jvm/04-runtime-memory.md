# Runtime Memory Areas

## Learning Objectives

After completing this chapter, you should be able to:

- Explain JVM Runtime Memory Areas.
- Explain Heap Memory.
- Explain Java Stack.
- Explain Method Area (Metaspace).
- Explain Program Counter Register.
- Explain Native Method Stack.
- Compare Heap vs Stack.
- Explain Stack Frames.
- Explain where objects, references and variables are stored.
- Explain memory allocation during method execution.

---

# Introduction

When a Java application starts, the JVM creates several memory regions known as **Runtime Data Areas**.

Each area has a specific responsibility during program execution.

Some memory areas are shared across all threads, while others are created separately for each thread.

---

# Runtime Memory Areas

The JVM creates five runtime memory areas.

```
                    JVM Memory

        +-------------------------------+
        |       Method Area             |
        |      (Metaspace)              |
        +-------------------------------+

        +-------------------------------+
        |            Heap               |
        +-------------------------------+

Thread-1          Thread-2          Thread-3

+---------+      +---------+      +---------+
| Stack   |      | Stack   |      | Stack   |
+---------+      +---------+      +---------+

+---------+      +---------+      +---------+
| PC Reg  |      | PC Reg  |      | PC Reg  |
+---------+      +---------+      +---------+

+---------+      +---------+      +---------+
| Native  |      | Native  |      | Native  |
| Stack   |      | Stack   |      | Stack   |
+---------+      +---------+      +---------+
```

---

# Runtime Memory Summary

| Memory Area | Shared | Stores |
|-------------|--------|--------|
| Heap | ✅ Yes | Objects and Arrays |
| Java Stack | ❌ No | Method calls, local variables, references |
| Method Area (Metaspace) | ✅ Yes | Class metadata, runtime constant pool, static members |
| Program Counter Register | ❌ No | Current JVM instruction |
| Native Method Stack | ❌ No | Native method execution |

---

# Heap Memory

Heap is the runtime memory area where **all Java objects and arrays are allocated**.

It is shared among all threads.

The Garbage Collector primarily manages the Heap.

---

## Characteristics

- Shared memory
- Stores objects
- Stores arrays
- Managed by Garbage Collector
- Largest JVM memory area

---

## Example

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

---

## Multiple Objects

```
Stack

employee1 ----------+

employee2 ------+   |

                |   |

                ▼   ▼

Heap

+----------------------+

Employee

id = 101

+----------------------+

Employee

id = 102

+----------------------+
```

---

# Java Stack

Each thread has its own Java Stack.

Every method invocation creates a new **Stack Frame**.

When the method completes, its Stack Frame is removed automatically.

---

## Stack Stores

- Local variables
- Method parameters
- Reference variables
- Return information
- Operand Stack

---

## Method Call Example

```java
main()

↓

firstMethod()

↓

secondMethod()
```

Stack

```
+----------------------+

secondMethod()

+----------------------+

firstMethod()

+----------------------+

main()

+----------------------+
```

When

```
secondMethod()

returns
```

```
+----------------------+

firstMethod()

+----------------------+

main()

+----------------------+
```

---

# Stack Frames

Every method call creates a new Stack Frame.

A Stack Frame contains:

- Local variables
- Parameters
- Operand Stack
- Return address

Example

```java
public void add(int a, int b){

    int sum = a + b;

}
```

Memory

```
Stack Frame

a

b

sum

Return Address
```

---

# Method Area (Metaspace)

The Method Area stores class-level information.

Since Java 8, it is implemented as **Metaspace**.

---

## Stores

- Class metadata
- Method metadata
- Runtime Constant Pool
- Static members

---

## Example

```java
class Employee{

    static String company = "OpenAI";

}
```

Memory

```
Metaspace

Employee.class

↓

company
```

Objects referenced by static variables still reside in the Heap.

---

# Program Counter Register

Every thread owns its own Program Counter Register.

It stores the address of the **next JVM instruction** to execute.

Think of it as an instruction pointer.

```
Instruction 1

↓

Instruction 2

↓

Instruction 3
```

The PC Register tracks which instruction comes next.

---

# Native Method Stack

The Native Method Stack supports execution of native methods written in languages such as:

- C
- C++

Examples include:

- Printer drivers
- Camera drivers
- Operating system APIs

---

# Heap vs Stack

| Heap | Stack |
|------|-------|
| Stores Objects | Stores Local Variables |
| Shared Across Threads | Thread-specific |
| Managed by GC | Automatically cleaned after method returns |
| Larger Memory | Smaller Memory |
| Dynamic Allocation | LIFO Allocation |
| Slower Access | Faster Access |

---

# Object Allocation Example

```java
Employee employee = new Employee();
```

```
Stack

employee

↓

Heap

Employee Object
```

The reference variable is stored in the Stack.

The actual object is stored in the Heap.

---

# Method Execution Example

```
main()

↓

login()

↓

validateUser()
```

During execution

```
Stack

+----------------------+

validateUser()

+----------------------+

login()

+----------------------+

main()

+----------------------+
```

After

```
validateUser()

returns
```

its Stack Frame is removed automatically.

---

# Memory Lifecycle

```
Program Starts

↓

JVM Starts

↓

Runtime Memory Created

↓

Objects Allocated in Heap

↓

Methods Create Stack Frames

↓

Methods Return

↓

Stack Frames Removed

↓

Objects Become Unreachable

↓

Garbage Collector Reclaims Heap Memory

↓

Program Ends
```

---

# Real-world Example

Spring Boot Application

```
Application Starts

↓

Application.class

↓

Metaspace

↓

Beans Created

↓

Heap

↓

HTTP Request

↓

Controller Method

↓

Stack Frame

↓

Method Returns

↓

Stack Frame Removed

↓

Unused Objects

↓

Garbage Collection
```

---

# Common Misconceptions

❌ Objects are stored in Stack.

Correct:

Objects are stored in Heap.

---

❌ Heap stores local variables.

Correct:

Local variables are stored inside Stack Frames.

---

❌ Every thread has its own Heap.

Correct:

Heap is shared across all threads.

---

❌ Static variables are stored in Heap.

Correct:

Static members are associated with the Method Area (Metaspace). If a static field references an object, that object is allocated in the Heap.

---

# Summary

Runtime Memory Areas include:

- Heap
- Java Stack
- Method Area (Metaspace)
- Program Counter Register
- Native Method Stack

Each memory area performs a different role during program execution.

Understanding these areas is essential for learning Garbage Collection, Memory Leaks and JVM Performance.

---

# Key Takeaways

- Heap stores objects and arrays.
- Java Stack stores method calls and local variables.
- Method Area stores class metadata.
- Every thread has its own Stack.
- Heap is shared by all threads.
- Every method invocation creates a new Stack Frame.
- Stack Frames are automatically removed after method execution.

---

# Next Chapter

**05-string-pool.md**