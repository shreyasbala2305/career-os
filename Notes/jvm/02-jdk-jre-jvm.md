# JDK vs JRE vs JVM

## Learning Objectives

After completing this chapter, you should be able to:

- Explain JDK, JRE and JVM.
- Understand how Java programs are compiled and executed.
- Explain the relationship between JDK, JRE and JVM.
- Describe the role of the Java compiler (`javac`).
- Explain why Java is platform-independent.
- Answer common interview questions related to JDK, JRE and JVM.

---

# Introduction

One of the most frequently asked Java interview questions is:

> **What is the difference between JDK, JRE and JVM?**

Although these terms are closely related, each has a different responsibility in the Java ecosystem.

Understanding their relationship is essential before learning JVM architecture and memory management.

---

# Java Program Execution Flow

Every Java program follows the same execution process.

```
Source Code (.java)

↓

javac

↓

Bytecode (.class)

↓

java

↓

JVM

↓

Machine Code

↓

CPU
```

Let's understand each component involved in this process.

---

# What is JDK?

**JDK (Java Development Kit)** is a complete software development kit used to develop Java applications.

It contains everything required to write, compile, debug and run Java programs.

---

# Components of JDK

```
JDK

│

├── JRE

├── javac

├── java

├── jar

├── javadoc

├── jdb

└── Development Tools
```

---

# Responsibilities of JDK

The JDK is responsible for:

- Writing Java applications
- Compiling Java source code
- Running Java applications
- Debugging programs
- Packaging applications
- Generating documentation

---

# Important JDK Tools

## javac

Compiles Java source code into bytecode.

```
Hello.java

↓

javac

↓

Hello.class
```

Example

```bash
javac Hello.java
```

---

## java

Starts the JVM and executes bytecode.

Example

```bash
java Hello
```

---

## jar

Packages Java applications into executable JAR files.

---

## javadoc

Generates API documentation from Java source code.

---

## jdb

Java Debugger.

---

# What is JRE?

**JRE (Java Runtime Environment)** provides everything required to run Java applications.

Unlike the JDK, it does not contain development tools.

---

# Components of JRE

```
JRE

│

├── JVM

├── Runtime Libraries

└── Java APIs
```

Notice that

```
javac
```

is **not** included.

---

# Responsibilities of JRE

The JRE is responsible for:

- Starting the JVM
- Providing Java runtime libraries
- Executing Java applications

It cannot compile Java source code.

---

# What is JVM?

**JVM (Java Virtual Machine)** is an abstract machine that executes Java bytecode.

The JVM converts platform-independent bytecode into platform-specific machine code.

---

# Responsibilities of JVM

The JVM is responsible for:

- Loading classes
- Verifying bytecode
- Managing runtime memory
- Executing bytecode
- Garbage collection
- Thread management
- Security verification
- Interacting with native libraries

---

# Relationship Between JDK, JRE and JVM

```
JDK

↓

Contains

↓

JRE

↓

Contains

↓

JVM
```

Or

```
JDK

├── JRE

│     └── JVM
```

This hierarchy is one of the most important concepts in Java.

---

# JDK vs JRE vs JVM

| Feature | JDK | JRE | JVM |
|----------|-----|-----|-----|
| Full Form | Java Development Kit | Java Runtime Environment | Java Virtual Machine |
| Purpose | Develop & Run | Run Applications | Execute Bytecode |
| Compiler (`javac`) | ✅ | ❌ | ❌ |
| JVM | ✅ | ✅ | Itself |
| Runtime Libraries | ✅ | ✅ | ❌ |
| Development Tools | ✅ | ❌ | ❌ |
| Garbage Collection | ✅ (through JVM) | ✅ (through JVM) | ✅ |

---

# Platform Independence

Java source code is compiled only once.

```
Hello.java

↓

javac

↓

Hello.class
```

The same bytecode can execute on different operating systems.

```
Bytecode

↓

Windows JVM

↓

Windows Machine Code

-------------------------

Bytecode

↓

Linux JVM

↓

Linux Machine Code

-------------------------

Bytecode

↓

macOS JVM

↓

macOS Machine Code
```

This is known as

> **Write Once, Run Anywhere (WORA)**

---

# Common Misconceptions

❌ JVM compiles Java code.

Correct:

`javac` compiles Java code.

The JVM executes bytecode.

---

❌ JRE contains the Java compiler.

Correct:

Only the JDK contains `javac`.

---

❌ JVM executes `.java` files.

Correct:

The JVM executes compiled `.class` files.

---

❌ Java is platform-independent because the JVM is platform-independent.

Correct:

The JVM implementation is platform-specific.

Java applications are platform-independent because the same bytecode runs on different JVM implementations.

---

# Summary

- JDK is used to develop Java applications.
- JRE is used to run Java applications.
- JVM executes Java bytecode.
- JDK contains JRE.
- JRE contains JVM.
- Java achieves platform independence through bytecode and JVM implementations.

---

# Key Takeaways

- JDK = Development + Runtime
- JRE = Runtime Environment
- JVM = Bytecode Execution Engine
- `javac` compiles source code.
- `java` starts the JVM.
- Bytecode is platform-independent.

---

# Next Chapter

**03-jvm-architecture.md**