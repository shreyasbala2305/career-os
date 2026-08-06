# JVM Architecture

## Learning Objectives

After completing this chapter, you should be able to:

- Explain the overall JVM architecture.
- Explain the role of each JVM component.
- Describe the complete execution flow inside the JVM.
- Explain how different JVM components interact.
- Draw the JVM architecture during an interview.
- Explain Interpreter and JIT Compiler.
- Explain JNI and Native Libraries.

---

# Introduction

The JVM (Java Virtual Machine) is responsible for executing Java bytecode.

It consists of several components that work together to load classes, allocate memory, execute bytecode, and manage application resources.

Understanding JVM architecture is fundamental for Java backend development and technical interviews.

---

# Complete JVM Architecture

```
                  Java Source Code

                         │

                      javac

                         │

                  Bytecode (.class)

                         │

                         ▼

            +----------------------------+
            |    Class Loader Subsystem  |
            +----------------------------+
                         │
                         ▼
            +----------------------------+
            |    Runtime Data Areas      |
            +----------------------------+
            | Heap                       |
            | Java Stack                 |
            | Method Area (Metaspace)    |
            | PC Register                |
            | Native Method Stack        |
            +----------------------------+
                         │
                         ▼
            +----------------------------+
            |     Execution Engine       |
            | Interpreter + JIT Compiler |
            +----------------------------+
                         │
                         ▼
            +----------------------------+
            |     Garbage Collector      |
            +----------------------------+
                         │
                         ▼
            +----------------------------+
            | Java Native Interface      |
            +----------------------------+
                         │
                         ▼
            +----------------------------+
            |    Native Libraries        |
            +----------------------------+
```

---

# JVM Execution Flow

Every Java program follows this execution flow.

```
Java Source Code

↓

javac

↓

.class

↓

JVM Starts

↓

Class Loader

↓

Runtime Memory

↓

Execution Engine

↓

Machine Code

↓

CPU Executes Instructions
```

---

# Components of JVM

The JVM consists of six major components.

```
1. Class Loader

2. Runtime Data Areas

3. Execution Engine

4. Garbage Collector

5. Java Native Interface (JNI)

6. Native Libraries
```

Each component has a specific responsibility.

---

# 1. Class Loader Subsystem

The Class Loader is responsible for loading compiled Java classes into JVM memory.

Without the Class Loader, the JVM cannot execute any Java program.

Responsibilities:

- Loading classes
- Linking classes
- Initializing classes

Execution Flow

```
.class

↓

Loading

↓

Linking

↓

Initialization
```

The Class Loading process is covered in detail in a later chapter.

---

# 2. Runtime Data Areas

Runtime Data Areas are the memory regions created by the JVM.

```
Runtime Data Areas

│

├── Heap

├── Java Stack

├── Method Area (Metaspace)

├── Program Counter Register

└── Native Method Stack
```

These memory areas store everything required during program execution.

The next chapter explains each memory area in detail.

---

# 3. Execution Engine

The Execution Engine is responsible for executing Java bytecode.

It consists of two major components.

```
Execution Engine

│

├── Interpreter

└── JIT Compiler
```

---

## Interpreter

The Interpreter executes bytecode one instruction at a time.

```
Bytecode

↓

Read Instruction

↓

Execute

↓

Next Instruction
```

Advantages

- Faster startup
- Simple execution

Disadvantages

- Slower for frequently executed code

---

## JIT Compiler

JIT (Just-In-Time Compiler) improves performance by compiling frequently executed bytecode into native machine code.

```
Frequently Executed Method

↓

JIT Compiler

↓

Native Machine Code

↓

Future Executions Become Faster
```

Advantages

- Better runtime performance
- Optimized machine code
- Faster repeated execution

---

## Interpreter vs JIT

| Interpreter | JIT Compiler |
|-------------|--------------|
| Executes bytecode line by line | Compiles frequently executed code |
| Faster startup | Faster execution |
| No optimization | Optimized native code |
| Used for all methods initially | Used for hot methods |

Modern JVMs use both together.

---

# 4. Garbage Collector

The Garbage Collector automatically removes objects that are no longer reachable.

Benefits

- Automatic memory management
- Reduces memory leaks
- Prevents manual memory deallocation

Garbage Collection is discussed in detail in a later chapter.

---

# 5. Java Native Interface (JNI)

JNI enables Java applications to communicate with native code written in languages such as:

- C
- C++

Examples

- Printer drivers
- Camera drivers
- Operating System APIs
- Hardware interaction

---

# 6. Native Libraries

Native Libraries are platform-specific libraries loaded through JNI.

Examples

```
Windows

↓

DLL

--------------------

Linux

↓

SO

--------------------

macOS

↓

DYLIB
```

---

# Component Interaction

```
Class Loader

↓

Loads Classes

↓

Runtime Memory

↓

Execution Engine

↓

Garbage Collector

↓

Native Calls (JNI)

↓

Native Libraries
```

Every Java application follows this sequence.

---

# Real-world Example

Suppose a Spring Boot application starts.

```
Application.class

↓

Class Loader

↓

Classes Loaded

↓

Beans Created

↓

Heap Allocation

↓

Execution Engine

↓

REST APIs Start Handling Requests

↓

Garbage Collector Removes Unused Objects
```

---

# Common Misconceptions

❌ JVM executes Java source code.

Correct:

The JVM executes compiled bytecode (`.class`).

---

❌ Interpreter is replaced by JIT.

Correct:

Modern JVMs use both Interpreter and JIT Compiler together.

---

❌ Heap stores local variables.

Correct:

Local variables are stored in Stack frames.

---

❌ Garbage Collector removes every unused object immediately.

Correct:

Garbage collection occurs when the JVM decides it is appropriate.

---

# Summary

The JVM architecture consists of:

- Class Loader
- Runtime Data Areas
- Execution Engine
- Garbage Collector
- Java Native Interface
- Native Libraries

Together, these components load classes, manage memory, execute bytecode, and interact with the operating system.

---

# Key Takeaways

- JVM executes Java bytecode.
- Class Loader loads classes into memory.
- Runtime Data Areas manage application memory.
- Execution Engine executes bytecode using Interpreter and JIT Compiler.
- Garbage Collector automatically reclaims unreachable objects.
- JNI enables interaction with native code.
- Native Libraries provide platform-specific functionality.

---

# Next Chapter

**04-runtime-memory.md**