# Chapter 4 – Queue

> "A Queue is a linear data structure that follows the FIFO (First-In First-Out) principle, where the first element inserted is the first one removed."

---

# What is a Queue?

## 🟢 Short Interview Answer (30–60 seconds)

A **Queue** is a collection designed for processing elements in the order they are inserted. It follows the **FIFO (First-In First-Out)** principle.

Unlike a List, elements are generally added at the rear and removed from the front.

The most commonly used Queue implementations are:

- LinkedList
- PriorityQueue
- ArrayDeque

---

# Why do we need Queue?

Imagine a ticket booking counter.

People arrive

```
Alice

↓

Bob

↓

Charlie
```

Who gets served first?

```
Alice
```

Because she arrived first.

This is

```
FIFO
```

---

# Real-world Examples

- Print Queue
- CPU Scheduling
- Job Queue
- Kafka Message Processing
- RabbitMQ Consumers
- Request Processing
- Order Processing
- BFS Graph Traversal

---

# Queue Hierarchy

```text
Iterable
      │
 Collection
      │
     Queue
      │
 ┌────┴─────────────┐
 │                  │
Deque         PriorityQueue
 │
 ├── ArrayDeque
 └── LinkedList
```

---

# Queue Characteristics

| Feature | Supported |
|----------|-----------|
| FIFO | ✅ |
| Duplicate Elements | ✅ |
| Null Values | Depends on implementation |
| Ordered | Processing Order |
| Thread Safe | Depends on implementation |

---

# Queue Operations

| Operation | Description |
|------------|-------------|
| offer() | Insert element |
| add() | Insert element |
| poll() | Remove front |
| remove() | Remove front |
| peek() | View front |
| element() | View front |

---

# Queue Example

```java
Queue<String> queue = new LinkedList<>();

queue.offer("Alice");
queue.offer("Bob");
queue.offer("Charlie");

System.out.println(queue.poll());

System.out.println(queue.peek());
```

Output

```
Alice

Bob
```

---

# Queue Visualization

```
Rear

↓

Charlie

↓

Bob

↓

Alice

↓

Front
```

Processing

```
poll()

↓

Alice

↓

Bob

↓

Charlie
```

---

# Queue Interface Methods

## offer()

Adds an element.

```java
queue.offer("Java");
```

Returns

```
true

or

false
```

---

## add()

Also inserts an element.

Difference

If insertion fails

```
offer()

↓

false
```

```
add()

↓

Exception
```

---

## poll()

Removes front element.

```java
queue.poll();
```

If queue is empty

Returns

```
null
```

---

## remove()

Also removes front.

Difference

If queue empty

Throws

```
NoSuchElementException
```

---

## peek()

Returns front element.

Does not remove it.

If queue empty

Returns

```
null
```

---

## element()

Same as peek()

Difference

Throws

```
NoSuchElementException
```

if queue is empty.

---

# offer() vs add()

| Feature | offer() | add() |
|-----------|----------|--------|
| Insert | Yes | Yes |
| Queue Full | false | Exception |

---

# poll() vs remove()

| Feature | poll() | remove() |
|-----------|----------|-----------|
| Removes Front | Yes | Yes |
| Empty Queue | null | Exception |

---

# peek() vs element()

| Feature | peek() | element() |
|-----------|----------|-------------|
| View Front | Yes | Yes |
| Empty Queue | null | Exception |

---

# Time Complexity

| Operation | Complexity |
|------------|------------|
| offer() | O(1) |
| poll() | O(1) |
| peek() | O(1) |

---

# Advantages

- FIFO processing
- Fast insertion
- Fast deletion
- Excellent for scheduling
- Ideal for producer-consumer systems

---

# Disadvantages

- No random access
- Cannot retrieve by index
- Sequential processing only

---

# Production Use Cases

✅ Task Scheduling

✅ Print Queue

✅ Customer Support Tickets

✅ Kafka Consumers

✅ RabbitMQ

✅ Order Processing

✅ Background Jobs

---

# Interview Questions

### Why Queue instead of List?

Queue models FIFO behavior directly, while List provides indexed access and ordering but not queue semantics.

---

### Why use offer() instead of add()?

`offer()` returns `false` when insertion fails instead of throwing an exception, making it safer in capacity-restricted queues.

---

### Which Queue implementation is used most?

For general-purpose queues:

```
ArrayDeque
```

For priority-based processing:

```
PriorityQueue
```

---

# Common Interview Mistakes

❌ Queue means sorted.

Wrong.

Queue processes elements based on insertion order unless using `PriorityQueue`.

---

❌ Queue supports indexing.

Wrong.

Queues are sequential collections.

---

# Key Takeaways

- Queue follows FIFO.
- Queue is ideal for sequential processing.
- Prefer `offer()`, `poll()`, and `peek()` for safer queue operations.
- Different implementations provide different ordering guarantees.

---

# PriorityQueue

## 🟢 Short Interview Answer (30–60 seconds)

`PriorityQueue` is a Queue implementation that orders elements according to **priority** rather than insertion order.

By default, it stores elements in **natural ascending order**.

Internally it uses a **Binary Heap**.

---

# Why PriorityQueue?

Imagine a hospital emergency room.

Patients arrive

```
Patient A (Normal)

Patient B (Critical)

Patient C (Serious)
```

Although Patient A arrived first,

Patient B gets treated first.

Processing depends on

```
Priority

NOT

Arrival Time
```

---

# Internal Structure

PriorityQueue internally uses

```
Binary Heap
```

Specifically

```
Min Heap
```

---

# Binary Heap

Example

```
          10
        /    \
      20      30
     /  \    /
   40   50 60
```

Notice

Parent is always

```
≤ Children
```

The smallest element remains at the root.

---

# Source Code Insight

Simplified JDK

```java
transient Object[] queue;

private int size;
```

Internally

```
PriorityQueue

↓

Object[]

↓

Binary Heap
```

---

# Adding an Element

```java
queue.offer(25);
```

Steps

```
Insert at End

↓

Heapify Up

↓

Swap Until Heap Property Restored
```

Example

Before

```
      10
     /  \
   20    30
```

Insert

```
5
```

Temporary

```
      10
     /   \
   20     30
  /
5
```

Heapify

```
       5
     /   \
   10     30
  /
20
```

---

# Removing an Element

```java
queue.poll();
```

Steps

```
Remove Root

↓

Move Last Element to Root

↓

Heapify Down

↓

Restore Heap
```

---

# Time Complexity

| Operation | Complexity |
|------------|------------|
| offer() | O(log n) |
| poll() | O(log n) |
| peek() | O(1) |
| contains() | O(n) |

---

# Example

```java
PriorityQueue<Integer> pq =
        new PriorityQueue<>();

pq.offer(40);
pq.offer(10);
pq.offer(20);
pq.offer(5);

while (!pq.isEmpty()) {
    System.out.println(pq.poll());
}
```

Output

```
5
10
20
40
```

---

# Max Heap

Default

```
Min Heap
```

To create Max Heap

```java
PriorityQueue<Integer> pq =
new PriorityQueue<>(Comparator.reverseOrder());
```

Output

```
40
20
10
5
```

---

# Production Use Cases

- Task Scheduler
- CPU Scheduling
- Dijkstra Algorithm
- A* Search
- Top K Problems
- Job Scheduling

---

# Common Mistakes

❌ PriorityQueue is sorted internally.

Wrong.

Only the root element is guaranteed to have the highest priority.

The remaining elements are organized as a heap, not as a fully sorted array.

---

# ArrayDeque

## 🟢 Short Interview Answer

`ArrayDeque` is a resizable-array implementation of the `Deque` interface.

It supports insertion and deletion from **both ends**.

It is the recommended replacement for

```
Stack

and

LinkedList
```

for stack/queue operations.

---

# Internal Structure

```
Circular Array
```

Unlike ArrayList

```
Normal Array
```

ArrayDeque uses

```
Head

↓

Tail
```

inside a circular buffer.

---

# Circular Array

```
+---+---+---+---+---+
| A | B | C |   |   |
+---+---+---+---+---+

Head

↓

A

Tail

↓

C
```

When reaching the end

```
Tail

↓

Wrap Around

↓

Beginning
```

No shifting.

---

# Source Code Insight

Simplified

```java
transient Object[] elements;

transient int head;

transient int tail;
```

---

# Why ArrayDeque is Fast?

Operations occur only at

```
Head

or

Tail
```

No shifting.

Therefore

```
offerFirst()

offerLast()

pollFirst()

pollLast()

↓

O(1)
```

---

# Example

```java
Deque<String> deque =
new ArrayDeque<>();

deque.offerFirst("Java");

deque.offerLast("Spring");

deque.offerFirst("Docker");

System.out.println(deque);
```

Output

```
[Docker, Java, Spring]
```

---

# Stack Using ArrayDeque

```java
Deque<String> stack =
new ArrayDeque<>();

stack.push("Java");

stack.push("Spring");

stack.push("Docker");

System.out.println(stack.pop());
```

Output

```
Docker
```

---

# Queue Using ArrayDeque

```java
Deque<String> queue =
new ArrayDeque<>();

queue.offer("Java");

queue.offer("Spring");

queue.offer("Docker");

System.out.println(queue.poll());
```

Output

```
Java
```

---

# Time Complexity

| Operation | Complexity |
|------------|------------|
| offerFirst() | O(1) |
| offerLast() | O(1) |
| pollFirst() | O(1) |
| pollLast() | O(1) |
| peekFirst() | O(1) |
| peekLast() | O(1) |

---

# Advantages

- Extremely fast
- No synchronization overhead
- Circular array
- Excellent cache locality
- Better than Stack
- Better than LinkedList for queue operations

---

# Disadvantages

- No indexed access
- No random lookup
- No null elements allowed

---

# Deque

## 🟢 Short Interview Answer

`Deque` (Double Ended Queue) allows insertion and removal from **both the front and rear**.

It can function as

- Queue (FIFO)
- Stack (LIFO)

---

# Internal Working

```
Front

↓

A

↓

B

↓

C

↓

Rear
```

Insertion

```
Front

or

Rear
```

Removal

```
Front

or

Rear
```

---

# Queue vs Deque

| Feature | Queue | Deque |
|----------|-------|--------|
| Insert Front | ❌ | ✅ |
| Insert Rear | ✅ | ✅ |
| Remove Front | ✅ | ✅ |
| Remove Rear | ❌ | ✅ |

---

# Production Use Cases

Queue

- Print Queue
- Kafka Consumer
- Order Processing

Deque

- Undo/Redo
- Browser History
- Sliding Window Algorithms
- LRU Cache (with LinkedHashMap)
- Expression Evaluation

---

# Interview Questions

### Why is ArrayDeque preferred over Stack?

Because Stack extends Vector and synchronizes every operation.

ArrayDeque is faster, modern, and designed specifically for stack/queue operations.

---

### Why is ArrayDeque faster than LinkedList?

- Better CPU cache locality
- No node allocation
- No previous/next references
- Lower memory overhead

---

### Which Queue implementation should I use?

| Requirement | Collection |
|-------------|------------|
| FIFO Queue | ArrayDeque |
| Priority Processing | PriorityQueue |
| Double Ended Queue | ArrayDeque |
| Legacy Queue | LinkedList |

---

# BlockingQueue (Introduction)

## 🟡 Short Interview Answer (30–60 seconds)

`BlockingQueue` is a thread-safe queue designed for concurrent programming. Unlike a normal Queue, it can **block** producer or consumer threads when the queue is full or empty.

It is heavily used in:

- Thread Pools
- Producer-Consumer Systems
- Task Scheduling
- Messaging Systems

---

# Why BlockingQueue?

Imagine an online food delivery system.

Chef

↓

Prepares Food

↓

Delivery Boy

↓

Delivers Food

What if

Delivery Boy arrives first?

```
Queue Empty
```

He should wait.

Similarly,

What if

Kitchen is Full?

Chef should wait.

This waiting behavior is exactly what BlockingQueue provides.

---

# Producer-Consumer Pattern

Producer

↓

Creates Task

↓

BlockingQueue

↓

Consumer

↓

Processes Task

Diagram

```
Producer

↓

BlockingQueue

↓

Consumer
```

Producer

```
offer()

↓

Queue
```

Consumer

```
take()

↓

Process
```

---

# Queue States

## Empty Queue

Consumer calls

```java
take()
```

Behavior

```
Wait

Until

Producer Inserts Data
```

---

## Full Queue

Producer calls

```java
put()
```

Behavior

```
Wait

Until

Consumer Removes Data
```

---

# Common Implementations

| Implementation | Description |
|----------------|-------------|
| ArrayBlockingQueue | Fixed Capacity Queue |
| LinkedBlockingQueue | Linked List Based Queue |
| PriorityBlockingQueue | Priority Queue |
| DelayQueue | Delayed Tasks |
| SynchronousQueue | Direct Thread Handoff |

---

# ArrayBlockingQueue

Uses

```
Circular Array
```

Characteristics

- Fixed Size
- Thread Safe
- Predictable Memory Usage

Example

```java
BlockingQueue<String> queue =
new ArrayBlockingQueue<>(10);
```

---

# LinkedBlockingQueue

Uses

```
Linked List
```

Characteristics

- Optional Capacity
- Better for dynamic workloads
- Higher memory usage

Example

```java
BlockingQueue<String> queue =
new LinkedBlockingQueue<>();
```

---

# Producer Example

```java
BlockingQueue<String> queue =
new LinkedBlockingQueue<>();

queue.put("Task-1");

queue.put("Task-2");
```

If the queue is full,

```
put()

↓

Wait
```

---

# Consumer Example

```java
String task = queue.take();

System.out.println(task);
```

If the queue is empty,

```
take()

↓

Wait
```

---

# Important Methods

| Method | Behavior |
|----------|----------|
| put() | Wait if full |
| take() | Wait if empty |
| offer() | Returns false if full |
| poll() | Returns null if empty |

---

# Queue Comparison

| Method | Waits? |
|----------|---------|
| put() | ✅ |
| offer() | ❌ |
| take() | ✅ |
| poll() | ❌ |

---

# Producer-Consumer Flow

```
Producer

↓

put()

↓

BlockingQueue

↓

take()

↓

Consumer
```

---

# Thread Safety

Normal Queue

```
Not Thread Safe
```

BlockingQueue

```
Thread Safe
```

Synchronization is handled internally.

---

# Real-world Backend Examples

## ThreadPoolExecutor

```
Tasks

↓

BlockingQueue

↓

Worker Threads
```

---

## Kafka Consumer

```
Kafka

↓

BlockingQueue

↓

Business Logic
```

---

## RabbitMQ

```
RabbitMQ

↓

BlockingQueue

↓

Consumer Thread
```

---

## Background Email Sending

```
User Registration

↓

Email Queue

↓

Background Worker

↓

Send Email
```

---

# Performance Comparison

| Collection | Internal Structure | add() | remove() | Thread Safe |
|-------------|--------------------|--------|-----------|-------------|
| LinkedList | Doubly Linked List | O(1) | O(1) | ❌ |
| ArrayDeque | Circular Array | O(1) | O(1) | ❌ |
| PriorityQueue | Binary Heap | O(log n) | O(log n) | ❌ |
| BlockingQueue | Depends on Implementation | O(1) / O(log n) | O(1) / O(log n) | ✅ |

---

# Queue Selection Guide

Need

```
FIFO

↓

ArrayDeque
```

Need

```
Priority Processing

↓

PriorityQueue
```

Need

```
Multiple Threads

↓

BlockingQueue
```

Need

```
Producer Consumer

↓

BlockingQueue
```

---

# Backend Interview Questions

## 🟢 Beginner

### What is Queue?

A FIFO collection where the first inserted element is the first removed.

---

### Difference between Queue and Deque?

Queue supports insertion at the rear and removal from the front.

Deque supports insertion and removal from both ends.

---

### Which Queue implementation is fastest?

For general-purpose FIFO operations,

```
ArrayDeque
```

---

## 🟡 Intermediate

### Why is PriorityQueue not FIFO?

Because elements are processed according to priority instead of insertion order.

---

### Why is ArrayDeque preferred over LinkedList?

- Better cache locality
- Lower memory overhead
- No node allocation
- Faster in practice for queue/stack operations

---

### Why doesn't PriorityQueue return sorted iteration?

Because it maintains a heap.

Only the root element is guaranteed to be the highest priority.

---

## 🔴 Advanced

### Explain Producer-Consumer Pattern.

A producer generates data and inserts it into a BlockingQueue.

Consumers remove data and process it.

BlockingQueue synchronizes producers and consumers automatically.

---

### Why BlockingQueue instead of Queue?

BlockingQueue provides built-in thread safety and blocking behavior, making it suitable for concurrent applications.

---

### Which Queue does ThreadPoolExecutor use?

Typically a `BlockingQueue` such as `LinkedBlockingQueue` or `ArrayBlockingQueue`, depending on the executor configuration.

---

# Common Interview Mistakes

❌ Queue is always sorted.

Wrong.

Only `PriorityQueue` orders elements by priority.

---

❌ ArrayDeque is synchronized.

Wrong.

It is **not** thread-safe.

---

❌ PriorityQueue internally uses a Tree.

Wrong.

It uses a **Binary Heap**.

---

❌ BlockingQueue is just another Queue.

Wrong.

It provides blocking operations and thread safety for concurrent programming.

---

# One-Day Revision Sheet

## Remember

✅ Queue → FIFO

✅ PriorityQueue → Binary Heap

✅ ArrayDeque → Circular Array

✅ BlockingQueue → Thread Safe

---

### Best Choices

| Requirement | Collection |
|--------------|------------|
| FIFO | ArrayDeque |
| Priority Scheduling | PriorityQueue |
| Producer-Consumer | BlockingQueue |
| Stack | ArrayDeque |
| Concurrent Task Queue | LinkedBlockingQueue |

---

# Chapter Summary

After completing this chapter, you should be able to:

- Explain Queue and FIFO processing.
- Compare Queue, Deque, and PriorityQueue.
- Describe the internal implementation of ArrayDeque and PriorityQueue.
- Explain the Producer–Consumer pattern.
- Understand the purpose of BlockingQueue.
- Select the appropriate queue implementation for backend applications.
- Answer common Java backend interview questions related to queues.

---

# Next Chapter

**05-map.md**

Topics:

- What is Map?
- HashMap
- LinkedHashMap
- TreeMap
- Hashtable
- ConcurrentHashMap
- HashMap Internals
- Hashing
- Buckets
- hashCode()
- equals()
- Collision Handling
- Load Factor
- Capacity
- Rehashing
- Treeification
- Java 8 Improvements
- Performance Analysis
- Production Decision Guide