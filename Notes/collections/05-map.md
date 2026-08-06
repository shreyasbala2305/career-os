# Chapter 5 – Map

> "A Map stores data as key-value pairs, enabling fast lookup, insertion, and deletion. It is one of the most frequently used data structures in Java backend development."

---

# What is a Map?

## 🟢 Short Interview Answer (30–60 seconds)

A **Map** is a data structure that stores data in **key-value pairs**.

Each key is unique and maps to exactly one value.

Unlike a Collection, a Map does **not** store individual elements.

Instead, it stores

```
Key

↓

Value
```

Examples

```
Employee ID

↓

Employee Object

Username

↓

Password

Country Code

↓

Country Name
```

---

# Why Do We Need a Map?

Suppose we store employees in a List.

```
Employee

↓

ID = 101

↓

Name = Alice
```

Finding Employee 101 requires searching the entire list.

Time

```
O(n)
```

Now using Map

```
101

↓

Alice
```

Lookup

```
O(1)
```

Average case.

---

# Real-world Examples

- User Sessions
- JWT Token Cache
- Employee Database
- Configuration Properties
- API Response Cache
- Shopping Cart
- Database Indexes
- Spring Bean Container

---

# Why is Map NOT Part of Collection?

This is a very common interview question.

Collection stores

```
Element

Element

Element
```

Map stores

```
Key

↓

Value
```

Because the storage model is different,

Map belongs to a separate hierarchy.

---

# Map Hierarchy

```text
                Map
                 │
     ┌───────────┼──────────────┐
     │           │              │
 HashMap   LinkedHashMap     Hashtable
     │
ConcurrentHashMap

TreeMap
```

---

# Common Map Implementations

| Implementation | Ordering | Thread Safe | Internal Structure |
|----------------|----------|-------------|--------------------|
| HashMap | No | ❌ | Hash Table |
| LinkedHashMap | Insertion Order | ❌ | Hash Table + Linked List |
| TreeMap | Sorted | ❌ | Red-Black Tree |
| Hashtable | No | ✅ | Hash Table |
| ConcurrentHashMap | No | ✅ | Concurrent Hash Table |

---

# Map Characteristics

| Feature | Supported |
|----------|-----------|
| Key-Value Pair | ✅ |
| Duplicate Keys | ❌ |
| Duplicate Values | ✅ |
| Null Key | Depends |
| Null Value | Depends |
| Fast Lookup | HashMap |

---

# Key Rules

Keys

```
Must Be Unique
```

Values

```
May Be Duplicate
```

Example

```java
Map<Integer,String> employees =
        new HashMap<>();

employees.put(101,"Alice");
employees.put(102,"Bob");
employees.put(103,"Alice");
```

Valid.

Because values may repeat.

---

# Duplicate Keys

```java
Map<Integer,String> map =
        new HashMap<>();

map.put(101,"Alice");

map.put(101,"Bob");
```

Output

```
101

↓

Bob
```

Old value replaced.

---

# Null Handling

| Collection | Null Key | Null Value |
|------------|----------|------------|
| HashMap | One | Multiple |
| LinkedHashMap | One | Multiple |
| TreeMap | ❌ | Yes |
| Hashtable | ❌ | ❌ |
| ConcurrentHashMap | ❌ | ❌ |

---

# HashMap

## 🟢 Short Interview Answer

HashMap is the most commonly used implementation of the Map interface.

It stores data in

```
Key

↓

Value
```

pairs.

Internally it uses

```
Hash Table

+

Buckets

+

Linked List

+

Red Black Tree (Java 8+)
```

It provides

Average

```
O(1)
```

lookup.

---

# Why HashMap?

Imagine

```
Employee ID

↓

Employee Object
```

Instead of searching every employee,

HashMap directly calculates

```
Bucket

↓

Retrieve Employee
```

Very fast.

---

# Basic Example

```java
Map<Integer,String> map =
        new HashMap<>();

map.put(101,"Alice");

map.put(102,"Bob");

map.put(103,"Charlie");

System.out.println(map.get(102));
```

Output

```
Bob
```

---

# Common Methods

| Method | Description |
|----------|-------------|
| put() | Insert or Update |
| get() | Retrieve Value |
| remove() | Delete Entry |
| containsKey() | Check Key |
| containsValue() | Check Value |
| keySet() | Get All Keys |
| values() | Get All Values |
| entrySet() | Get Key-Value Pairs |
| size() | Number of Entries |
| clear() | Remove Everything |

---

# put()

```java
map.put(1,"Java");
```

If key exists

↓

Replace value.

---

# get()

```java
map.get(1);
```

Returns

```
Java
```

---

# remove()

```java
map.remove(1);
```

Deletes

```
Key

+

Value
```

---

# containsKey()

```java
map.containsKey(100);
```

Returns

```
true

or

false
```

---

# containsValue()

```java
map.containsValue("Java");
```

Checks values.

Slower than

```
containsKey()
```

because values require linear search.

---

# keySet()

Returns

```
Set<K>
```

Example

```java
for(Integer id : map.keySet()){

    System.out.println(id);

}
```

---

# values()

Returns

```
Collection<V>
```

---

# entrySet()

Most efficient iteration.

```java
for(Map.Entry<Integer,String> entry : map.entrySet()){

    System.out.println(entry.getKey());

    System.out.println(entry.getValue());

}
```

---

# Time Complexity

| Operation | Average | Worst |
|------------|---------|--------|
| put() | O(1) | O(n) |
| get() | O(1) | O(n) |
| remove() | O(1) | O(n) |
| containsKey() | O(1) | O(n) |
| containsValue() | O(n) | O(n) |

---

# Real-world Backend Examples

## Session Management

```
Session ID

↓

User Session
```

---

## Spring Bean Container

```
Bean Name

↓

Bean Object
```

---

## JWT Cache

```
Token

↓

User Details
```

---

## Employee Repository

```
Employee ID

↓

Employee
```

---

# Common Interview Questions

### Why are duplicate keys not allowed?

Because every key uniquely identifies a value. If the same key is inserted again, the existing value is replaced.

---

### Why are duplicate values allowed?

Values are not used for lookup, so multiple keys can reference the same value.

---

### Which method is faster: containsKey() or containsValue()?

`containsKey()`.

It uses hashing and performs an average O(1) lookup, while `containsValue()` scans values linearly.

---

# Common Interview Mistakes

❌ Map is a Collection.

Wrong.

Map belongs to the Java Collections Framework but is **not** part of the `Collection` interface hierarchy.

---

❌ HashMap stores objects randomly.

Wrong.

Entries are organized into buckets using hashing.

---

# Key Takeaways

- Map stores unique keys and associated values.
- HashMap is the most commonly used Map implementation.
- Duplicate keys replace existing values.
- Duplicate values are allowed.
- Average lookup is O(1).

---

# HashMap Internals (Most Important Topic)

> ⭐ This topic is one of the most frequently asked questions in Java Backend interviews.

Interviewers often ask:

- Explain HashMap internals.
- How does put() work?
- How does get() work?
- Why is HashMap O(1)?
- Explain buckets.
- Explain hashing.

If you understand this chapter, you'll answer all of them confidently.

---

# Internal Structure of HashMap

HashMap is **not** just a normal array.

Internally it consists of:

```
HashMap

↓

Array

↓

Bucket

↓

Node

↓

Key

↓

Value
```

A simplified view:

```
table[]

↓

+-------+
|Bucket0|
+-------+
|Bucket1|
+-------+
|Bucket2|
+-------+
|Bucket3|
+-------+
|Bucket4|
+-------+
```

Each bucket can store one or more nodes.

---

# JDK Source Code

Simplified

```java
transient Node<K,V>[] table;
```

This is the heart of HashMap.

It stores an **array of Nodes**.

---

# What is a Node?

Each bucket stores Nodes.

Simplified JDK source:

```java
static class Node<K,V> implements Map.Entry<K,V>{

    final int hash;

    final K key;

    V value;

    Node<K,V> next;

}
```

Each Node stores

```
Hash Value

↓

Key

↓

Value

↓

Next Node
```

---

# Visual Representation

Suppose we insert

```
101 → Alice

102 → Bob

103 → Charlie
```

Internally

```
table[]

↓

Bucket 0

↓

101

↓

Alice

↓

next

↓

null

----------------

Bucket 1

↓

102

↓

Bob

↓

null

----------------

Bucket 2

↓

103

↓

Charlie
```

---

# What is Hashing?

## 🟢 Interview Answer

Hashing is the process of converting a key into an integer hash value so that Java can quickly determine where to store or retrieve the corresponding value.

Instead of searching every element,

HashMap computes

```
Key

↓

hashCode()

↓

Bucket Index

↓

Store/Retrieve
```

---

# Why Hashing?

Imagine searching

```
1 Million Employees
```

Without hashing

```
Employee 1

↓

Employee 2

↓

Employee 3

...

↓

Employee 1,000,000
```

Time

```
O(n)
```

With hashing

```
Employee ID

↓

hashCode()

↓

Bucket 27

↓

Done
```

Time

```
O(1)
```

Average.

---

# Step 1 : hashCode()

Suppose

```java
map.put("Java",100);
```

Java first calls

```java
"Java".hashCode();
```

Suppose

```
2301506
```

is returned.

This is **not** the bucket index.

It is only the hash value.

---

# Step 2 : Improve the Hash

HashMap does not use the raw hashCode directly.

Simplified JDK:

```java
hash = key.hashCode();

hash = hash ^ (hash >>> 16);
```

This mixes the higher and lower bits.

Why?

To distribute keys more evenly across buckets and reduce collisions.

---

# Step 3 : Calculate Bucket Index

Formula

```java
index = (table.length - 1) & hash;
```

Example

Capacity

```
16
```

Hash

```
2301506
```

Bucket

```
2301506 & 15

↓

Bucket 2
```

The key is stored in Bucket 2.

---

# Why use '&' instead of '%'?

Interview Question

Many candidates answer:

```
hash % capacity
```

Wrong for modern HashMap.

Java uses

```java
(hash & (capacity - 1))
```

because:

- Faster than modulus (`%`)
- Works efficiently when capacity is a power of two
- Uses bitwise operations, which are CPU-friendly

---

# Why is Capacity Always a Power of Two?

Interview Question

Default capacity:

```
16
```

Then

```
32

↓

64

↓

128

↓

256
```

Never

```
15

30

50
```

Reason:

Only powers of two allow the bitwise AND operation to distribute keys uniformly.

If capacity were 15:

```
hash & 14
```

Many buckets would never be used efficiently.

---

# Default Capacity

```java
new HashMap<>();
```

Creates

```
Capacity = 16
```

(Default table size after first insertion.)

---

# Visual Example

Capacity

```
16 Buckets
```

```
0

1

2

3

4

...

15
```

Insert

```
Java
```

↓

```
hashCode()

↓

Bucket 7
```

Insert

```
Spring
```

↓

```
hashCode()

↓

Bucket 12
```

Insert

```
Docker
```

↓

```
hashCode()

↓

Bucket 4
```

Each key goes directly to its calculated bucket.

---

# How put() Works

Interview Favorite ⭐

Suppose

```java
map.put(101,"Alice");
```

Execution Flow

```
put()

↓

Calculate hashCode()

↓

Improve hash

↓

Find Bucket

↓

Bucket Empty?

↓

YES

↓

Insert Node

↓

Done
```

If bucket already contains nodes

↓

Handle Collision

(covered in next section)

---

# How get() Works

Suppose

```java
map.get(101);
```

Execution

```
Key

↓

hashCode()

↓

Improve hash

↓

Find Bucket

↓

Traverse Bucket

↓

equals()

↓

Return Value
```

Notice

HashMap **never searches the entire table**.

It directly jumps to one bucket.

---

# Why is HashMap Fast?

Without HashMap

```
Search

↓

Every Object

↓

O(n)
```

With HashMap

```
hashCode()

↓

Bucket

↓

1–2 Comparisons

↓

Done

↓

O(1)
```

Average.

---

# Complexity

| Operation | Average | Worst |
|-----------|---------|--------|
| put() | O(1) | O(n) |
| get() | O(1) | O(n) |
| remove() | O(1) | O(n) |

Worst case occurs when many keys end up in the same bucket.

(Java 8 improves this using treeification, which we'll cover next.)

---

# Real-world Example

Imagine a library.

Without hashing:

```
Walk through every shelf

↓

Find Book
```

With hashing:

```
Book ID

↓

Shelf Number

↓

Go directly to shelf

↓

Find Book
```

HashMap works the same way.

---

# Common Interview Mistakes

❌ HashMap stores data randomly.

Wrong.

It stores data in **calculated buckets**.

---

❌ hashCode() returns the bucket index.

Wrong.

`hashCode()` returns a hash value.

HashMap calculates the bucket index from that hash.

---

❌ HashMap uses modulus (%) internally.

Modern HashMap uses:

```java
(hash & (capacity - 1))
```

---

# Interview Cheat Sheet

| Question | Answer |
|----------|--------|
| Internal structure | Array of Nodes |
| Node stores | Hash, Key, Value, Next |
| Storage | Buckets |
| Lookup | hashCode() → Bucket → equals() |
| Default Capacity | 16 |
| Capacity Growth | Doubles (16 → 32 → 64...) |
| Index Formula | `(n - 1) & hash` |
| Average Lookup | O(1) |
| Worst Case | O(n) |

---

---

# Collision Handling

⭐⭐⭐ One of the most asked Java Backend interview questions.

## What is a Collision?

### 🟢 Short Interview Answer (30–60 seconds)

A collision occurs when **two different keys produce the same bucket index** in a HashMap.

Since both keys cannot occupy the exact same position in the bucket array, HashMap stores them together using a collision resolution strategy.

---

## Example

Suppose

```
HashMap Capacity = 16
```

Keys

```
"Java"

↓

Bucket 5

"Spring"

↓

Bucket 5
```

Both keys point to

```
Bucket 5
```

This is called a

```
Collision
```

---

## Why do collisions happen?

HashMap has a limited number of buckets.

Imagine

```
1 Million Keys

↓

Only 16 Buckets
```

Multiple keys **must** share buckets.

Collisions are unavoidable.

---

# Separate Chaining

Java resolves collisions using

```
Separate Chaining
```

---

## Internal Structure

Suppose

```
Bucket 7
```

contains

```
Java

Spring

Docker
```

Internally

```
Bucket 7

↓

Java

↓

Spring

↓

Docker

↓

null
```

Every bucket stores

```
Linked List of Nodes
```

(Java 8 may convert it into a tree.)

---

# Collision Resolution Process

Insert

```
Java
```

↓

Bucket 4

↓

Empty

↓

Insert

----------------------

Insert

```
Spring
```

↓

Bucket 4

↓

Existing Node

↓

equals()

↓

Different Key

↓

Append Node

---

Diagram

```
Bucket 4

↓

Java

↓

Spring

↓

Docker
```

---

# How get() Works During Collision

Suppose

```
Bucket 4

↓

Java

↓

Spring

↓

Docker
```

Retrieve

```
Docker
```

Execution

```
hashCode()

↓

Bucket 4

↓

Java ?

↓

No

↓

Spring ?

↓

No

↓

Docker ?

↓

Yes

↓

Return Value
```

Notice

HashMap searches

```
ONLY ONE BUCKET
```

not the entire table.

---

# Time Complexity During Collision

Small bucket

```
Java

↓

Spring
```

Search

```
O(2)
```

Large bucket

```
100 Nodes
```

Search

```
O(100)

↓

O(n)
```

Hence worst case

```
O(n)
```

---

# Capacity

Interview Favorite ⭐

## What is Capacity?

Capacity means

```
Number of Buckets
```

Default

```
16
```

Example

```
Bucket

0

1

2

...

15
```

Total

```
16 Buckets
```

---

# Default Capacity

```java
Map<Integer,String> map =
new HashMap<>();
```

Initially

```
Capacity = 16
```

(after first insertion)

---

# Load Factor

⭐⭐⭐ Extremely Important

## What is Load Factor?

### Interview Answer

Load Factor determines **how full a HashMap is allowed to become before resizing itself**.

Default

```
0.75
```

---

Formula

```
Load Factor

=

Size

/

Capacity
```

---

Example

Capacity

```
16
```

Load Factor

```
0.75
```

Threshold

```
16 × 0.75

=

12
```

After inserting

```
13th Element
```

HashMap resizes.

---

# Why 0.75?

Interview Question

Why not

```
1.0 ?
```

Too many collisions.

---

Why not

```
0.25 ?
```

Too much unused memory.

---

Java designers found

```
0.75
```

to be the best balance between

- Memory
- Performance

---

# Threshold

Threshold means

```
Maximum Entries

Before Resize
```

Formula

```
Threshold

=

Capacity × Load Factor
```

Example

| Capacity | Threshold |
|----------|-----------|
| 16 | 12 |
| 32 | 24 |
| 64 | 48 |

---

# Rehashing

⭐⭐⭐ Very Frequently Asked

## What is Rehashing?

Rehashing is the process of

```
Creating a Larger Bucket Array

↓

Recalculating Bucket Index

↓

Moving Every Entry
```

---

## Example

Before

```
Capacity

16
```

Insert

```
13th Element
```

Threshold exceeded.

New Capacity

```
32
```

Every key is inserted again into the new bucket array.

---

Diagram

Old Table

```
16 Buckets
```

↓

Create

```
32 Buckets
```

↓

Recalculate

↓

Move Every Entry

---

# Why Rehash?

Suppose

```
16 Buckets

↓

200 Entries
```

Collisions increase.

Performance decreases.

Rehashing spreads entries across more buckets.

---

# Capacity Growth

```
16

↓

32

↓

64

↓

128

↓

256
```

Capacity always doubles.

---

# Treeification (Java 8+)

⭐⭐⭐⭐⭐ Most Important Interview Topic

Before Java 8

Collision

↓

Linked List

Only

---

Example

```
Bucket

↓

A

↓

B

↓

C

↓

D

↓

E

↓

F

↓

G

↓

H
```

Searching

```
O(n)
```

---

Java 8 Improvement

If

```
Bucket Size ≥ 8

AND

Capacity ≥ 64
```

Linked List

↓

Converted Into

↓

Red-Black Tree

---

Diagram

Before

```
A

↓

B

↓

C

↓

D

↓

E
```

After

```
      D

    /   \

   B     F

  / \   / \

 A  C  E  G
```

Searching

```
O(log n)
```

instead of

```
O(n)
```

---

# Untreeification

If bucket size falls below

```
6
```

Tree

↓

Converted Back

↓

Linked List

This avoids unnecessary tree overhead.

---

# Why Average Lookup is O(1)?

Because

```
hashCode()

↓

Bucket

↓

Very Few Comparisons

↓

Done
```

Only one bucket is searched.

---

# Why Worst Case Becomes O(n)?

Suppose

Every key lands in

```
Bucket 5
```

```
A

↓

B

↓

C

↓

D

↓

E
```

Now every lookup traverses the bucket.

Time

```
O(n)
```

---

# Java 8 Improvement

Before Java 8

Worst

```
O(n)
```

After Java 8

Large bucket

↓

Red-Black Tree

↓

Worst

```
O(log n)
```

---

# Java 7 vs Java 8

| Feature | Java 7 | Java 8 |
|----------|---------|---------|
| Collision Handling | Linked List | Linked List + Red-Black Tree |
| Worst Case | O(n) | O(log n) (after treeification) |
| Performance | Lower | Better |

---

# Interview Cheat Sheet

| Topic | Key Point |
|--------|-----------|
| Collision | Same bucket for different keys |
| Resolution | Separate Chaining |
| Capacity | Number of buckets |
| Default Capacity | 16 |
| Load Factor | 0.75 |
| Threshold | Capacity × Load Factor |
| Rehashing | Double capacity and redistribute entries |
| Treeification | Bucket size ≥ 8 and capacity ≥ 64 |
| Untreeification | Bucket size < 6 |
| Average Lookup | O(1) |
| Worst Lookup | O(n), improved to O(log n) after treeification |

---

# Common Interview Mistakes

❌ Collision means duplicate key.

Wrong.

Different keys can produce the same bucket index.

---

❌ Load Factor means bucket size.

Wrong.

It measures how full the entire HashMap is.

---

❌ Rehashing only increases capacity.

Wrong.

It **also recalculates the bucket index for every entry**.

---

❌ Treeification happens whenever there are 8 elements in the map.

Wrong.

Treeification occurs only when:

- Bucket size ≥ 8
- Table capacity ≥ 64

Otherwise, HashMap resizes instead of converting to a tree.

---

# LinkedHashMap

## 🟢 Short Interview Answer (30–60 seconds)

`LinkedHashMap` is an extension of `HashMap` that maintains the **insertion order** of entries.

Internally, it uses a **HashMap + Doubly Linked List**.

It provides nearly the same performance as `HashMap` while preserving predictable iteration order.

---

# Why LinkedHashMap?

Suppose we insert

```java
map.put(101, "Alice");
map.put(102, "Bob");
map.put(103, "Charlie");
```

Output

```
101 -> Alice
102 -> Bob
103 -> Charlie
```

The insertion order is preserved.

Unlike HashMap, iteration order is predictable.

---

# Internal Structure

```
LinkedHashMap

↓

Hash Table

+

Doubly Linked List
```

Visualization

```
Buckets

↓

101

↓

102

↓

103

-------------------

Linked List

101 ⇄ 102 ⇄ 103
```

The linked list preserves insertion order.

---

# Source Code Insight

Simplified JDK Node

```java
static class Entry<K,V> extends HashMap.Node<K,V>{

    Entry<K,V> before;

    Entry<K,V> after;

}
```

Compared to HashMap's Node, LinkedHashMap adds two references:

- before
- after

---

# Access Order Mode

LinkedHashMap supports two modes:

### Insertion Order (Default)

```java
new LinkedHashMap<>();
```

Iteration follows insertion order.

---

### Access Order

```java
new LinkedHashMap<>(16, 0.75f, true);
```

Whenever an entry is accessed, it moves to the end of the linked list.

This feature is useful for implementing **LRU (Least Recently Used) Cache**.

---

# Time Complexity

| Operation | Complexity |
|------------|------------|
| put() | O(1) |
| get() | O(1) |
| remove() | O(1) |

---

# Advantages

- Predictable iteration order
- Fast lookup
- Supports LRU cache implementation

---

# Disadvantages

- Slightly more memory than HashMap
- Slightly slower due to linked list maintenance

---

# Production Use Cases

- API responses
- JSON serialization
- Configuration maps
- LRU Cache
- Ordered reports

---

# TreeMap

## 🟢 Short Interview Answer

TreeMap stores key-value pairs in **sorted order**.

Internally, it uses a **Red-Black Tree**.

Unlike HashMap, it does not use hashing.

All operations are **O(log n)**.

---

# Internal Structure

```
TreeMap

↓

Red-Black Tree
```

Example

```
        50
      /    \
    30      70
   /  \    /  \
 20  40  60   80
```

Keys are automatically sorted.

---

# Sorting

```java
TreeMap<Integer, String> map =
        new TreeMap<>();

map.put(3, "C");
map.put(1, "A");
map.put(2, "B");
```

Output

```
1=A
2=B
3=C
```

Sorting is based on:

- Natural Ordering (`Comparable`)
- Custom `Comparator`

---

# Time Complexity

| Operation | Complexity |
|------------|------------|
| put() | O(log n) |
| get() | O(log n) |
| remove() | O(log n) |

---

# Advantages

- Automatically sorted keys
- Range queries
- Navigational methods

---

# Disadvantages

- Slower than HashMap
- No null keys
- More memory overhead

---

# Production Use Cases

- Leaderboards
- Reports
- Rankings
- Scheduling
- Sorted indexes

---

# Hashtable

## 🟢 Short Interview Answer

Hashtable is a **legacy**, synchronized implementation of the Map interface.

It is thread-safe but slower than HashMap due to synchronization.

Modern applications generally prefer `ConcurrentHashMap`.

---

# Characteristics

| Feature | Supported |
|----------|-----------|
| Thread Safe | ✅ |
| Null Key | ❌ |
| Null Value | ❌ |
| Legacy | ✅ |

---

# Internal Structure

```
Hashtable

↓

Hash Table

↓

Synchronized Methods
```

Every public method acquires a lock.

---

# Why is Hashtable Slower?

```java
public synchronized V put(...)
```

Every thread must wait for the lock.

This reduces concurrency.

---

# ConcurrentHashMap

⭐⭐⭐⭐ Very Important

## 🟢 Short Interview Answer

ConcurrentHashMap is a high-performance, thread-safe implementation of Map designed for concurrent applications.

Unlike Hashtable, it allows multiple threads to read and update different parts of the map simultaneously.

---

# Why ConcurrentHashMap?

Imagine

```
100 Threads

↓

Accessing Same Map
```

Hashtable

```
One Thread

↓

Lock Entire Map

↓

Others Wait
```

ConcurrentHashMap

```
Thread 1

↓

Bucket A

Thread 2

↓

Bucket B

Both Work Simultaneously
```

Much better performance.

---

# Java 8 Internal Working

Before Java 8

```
Segmented Locks
```

Java 8

```
Node[]

↓

CAS

+

Fine-Grained Synchronization
```

Instead of locking the whole map, it synchronizes only the bucket being modified.

---

# Null Handling

Why are null keys and values not allowed?

Suppose

```java
map.get(key)
```

returns

```
null
```

Does it mean

- key doesn't exist?

OR

- value is actually null?

Ambiguous.

To avoid this confusion, ConcurrentHashMap disallows null keys and values.

---

# Time Complexity

| Operation | Complexity |
|------------|------------|
| put() | O(1) Average |
| get() | O(1) Average |
| remove() | O(1) Average |

---

# Production Use Cases

- Spring Boot caches
- Session management
- Multi-threaded applications
- Thread pools
- High-concurrency services

---

# Complete Comparison

| Feature | HashMap | LinkedHashMap | TreeMap | Hashtable | ConcurrentHashMap |
|----------|----------|---------------|----------|-----------|-------------------|
| Ordering | None | Insertion | Sorted | None | None |
| Thread Safe | ❌ | ❌ | ❌ | ✅ | ✅ |
| Null Key | 1 | 1 | ❌ | ❌ | ❌ |
| Null Value | Multiple | Multiple | Yes | ❌ | ❌ |
| Internal Structure | Hash Table | Hash Table + Linked List | Red-Black Tree | Hash Table | Concurrent Hash Table |
| Average Lookup | O(1) | O(1) | O(log n) | O(1) | O(1) |

---

# Production Decision Guide

| Requirement | Recommended Collection |
|-------------|------------------------|
| Fastest Lookup | HashMap |
| Maintain Order | LinkedHashMap |
| Sorted Keys | TreeMap |
| Legacy Thread Safety | Hashtable |
| Modern Thread Safety | ConcurrentHashMap |

---

# Common Interview Mistakes

❌ Hashtable and ConcurrentHashMap are the same.

Wrong.

Hashtable locks the entire table.

ConcurrentHashMap uses fine-grained synchronization.

---

❌ LinkedHashMap is sorted.

Wrong.

It only preserves insertion order.

---

❌ TreeMap uses hashing.

Wrong.

It uses a Red-Black Tree.

---

# Interview Questions

### Why use LinkedHashMap instead of HashMap?

When insertion order or access order must be preserved.

---

### Why is TreeMap slower than HashMap?

Because TreeMap performs tree operations (O(log n)), while HashMap uses hashing (O(1) average).

---

### Why is ConcurrentHashMap preferred over Hashtable?

Because it provides better concurrency and scalability by reducing lock contention.

---

# LRU Cache using LinkedHashMap

⭐⭐⭐⭐ Frequently Asked (2–5 Years Experience)

## What is an LRU Cache?

LRU stands for

```
Least Recently Used
```

When the cache becomes full,

```
Remove

↓

Least Recently Used Entry
```

instead of the oldest inserted entry.

---

## Why LinkedHashMap?

LinkedHashMap supports

```
Access Order
```

which automatically moves recently accessed entries to the end.

Constructor

```java
new LinkedHashMap<>(16,0.75f,true);
```

Notice

```
true

↓

Access Order
```

instead of insertion order.

---

## Simple LRU Cache

```java
public class LRUCache<K,V>
        extends LinkedHashMap<K,V>{

    private final int capacity;

    public LRUCache(int capacity){

        super(capacity,0.75f,true);

        this.capacity=capacity;

    }

    @Override
    protected boolean removeEldestEntry(
            Map.Entry<K,V> eldest){

        return size()>capacity;

    }

}
```

---

## Example

```java
LRUCache<Integer,String> cache =
        new LRUCache<>(3);

cache.put(1,"Java");
cache.put(2,"Spring");
cache.put(3,"Docker");

cache.get(1);

cache.put(4,"Kafka");
```

Removed

```
2

↓

Spring
```

because it was the least recently used entry.

---

# Scenario-Based Interview Questions

## Scenario 1

Requirement

```
Store Employee by ID
```

Choose

```
HashMap
```

Reason

Fast O(1) lookup.

---

## Scenario 2

Requirement

```
Maintain insertion order
```

Choose

```
LinkedHashMap
```

---

## Scenario 3

Requirement

```
Sorted report
```

Choose

```
TreeMap
```

---

## Scenario 4

Requirement

```
Thread-safe cache
```

Choose

```
ConcurrentHashMap
```

---

## Scenario 5

Requirement

```
Implement LRU Cache
```

Choose

```
LinkedHashMap
```

---

## Scenario 6

Requirement

```
High-concurrency session storage
```

Choose

```
ConcurrentHashMap
```

---

# 40+ Interview Questions

## 🟢 Beginner

### What is Map?

Stores data as key-value pairs.

---

### Why are duplicate keys not allowed?

A key uniquely identifies its value.

---

### Are duplicate values allowed?

Yes.

---

### Which Map implementation is used most?

HashMap.

---

### Does HashMap maintain insertion order?

No.

---

### Does LinkedHashMap maintain insertion order?

Yes.

---

### Does TreeMap sort keys?

Yes.

---

### Does HashMap allow null?

One null key and multiple null values.

---

## 🟡 Intermediate

### Difference between HashMap and Hashtable?

| HashMap | Hashtable |
|----------|-----------|
| Not synchronized | Synchronized |
| Allows null | Doesn't allow null |
| Faster | Slower |

---

### Difference between HashMap and TreeMap?

| HashMap | TreeMap |
|----------|----------|
| O(1) | O(log n) |
| Hash Table | Red-Black Tree |
| No Ordering | Sorted |

---

### Difference between HashMap and LinkedHashMap?

LinkedHashMap maintains insertion order.

HashMap does not.

---

### Why is containsKey() faster than containsValue()?

`containsKey()` uses hashing.

`containsValue()` scans all values.

---

### What is hashing?

Converting a key into a hash value to locate a bucket efficiently.

---

### What are buckets?

Buckets are positions in the internal array where entries are stored.

---

### What is a collision?

Two different keys mapping to the same bucket.

---

### How are collisions handled?

Using Separate Chaining.

In Java 8+, long chains can become Red-Black Trees.

---

### What is Load Factor?

Defines how full a HashMap can become before resizing.

Default

```
0.75
```

---

### What is Capacity?

Number of buckets.

---

### What is Threshold?

```
Capacity × Load Factor
```

---

### What is Rehashing?

Creating a larger bucket array and redistributing entries.

---

### Why does HashMap resize?

To reduce collisions and maintain O(1) average performance.

---

## 🔴 Advanced

### Explain HashMap internal working.

1. Calculate `hashCode()`
2. Mix hash bits
3. Calculate bucket index
4. Search bucket
5. Use `equals()` to compare keys
6. Insert or update node

---

### Why is capacity always a power of two?

To allow fast bucket calculation using:

```java
(hash & (capacity - 1))
```

instead of modulus.

---

### Why does HashMap use `hash ^ (hash >>> 16)`?

To improve hash distribution and reduce collisions.

---

### Explain treeification.

If a bucket contains at least **8 nodes** and the table capacity is at least **64**, the linked list is converted into a Red-Black Tree.

---

### Explain untreeification.

If the bucket size drops below **6**, the tree is converted back into a linked list.

---

### Why is average lookup O(1)?

Hashing narrows the search to a single bucket.

---

### Why is worst-case lookup O(n)?

If many keys end up in the same bucket, the linked list must be traversed.

(Java 8 reduces this to O(log n) after treeification.)

---

### Why must equals() and hashCode() be overridden together?

To ensure logically equal objects are stored and retrieved correctly.

---

### Why doesn't ConcurrentHashMap allow null?

Because `null` would make it impossible to distinguish between:

- Key not present
- Key present with a null value

---

# Common Interview Mistakes

❌ HashMap is always O(1).

Wrong.

Worst case is O(n) (or O(log n) after treeification).

---

❌ Collision means duplicate key.

Wrong.

Different keys can hash to the same bucket.

---

❌ TreeMap uses hashing.

Wrong.

It uses a Red-Black Tree.

---

❌ LinkedHashMap is sorted.

Wrong.

It preserves insertion order only.

---

❌ Hashtable is recommended for modern applications.

Wrong.

Prefer `ConcurrentHashMap`.

---

# Complete Decision Matrix

| Requirement | Recommended Collection |
|-------------|------------------------|
| Fastest Lookup | HashMap |
| Maintain Insertion Order | LinkedHashMap |
| Sorted Keys | TreeMap |
| Thread Safety | ConcurrentHashMap |
| Legacy Code | Hashtable |
| LRU Cache | LinkedHashMap |
| High-Concurrency Cache | ConcurrentHashMap |

---

# One-Day Revision Sheet

## Remember

✅ HashMap → Hash Table

✅ LinkedHashMap → Hash Table + Doubly Linked List

✅ TreeMap → Red-Black Tree

✅ Hashtable → Legacy synchronized map

✅ ConcurrentHashMap → Thread-safe modern map

---

### Complexity

| Collection | put() | get() | remove() |
|------------|--------|--------|----------|
| HashMap | O(1) | O(1) | O(1) |
| LinkedHashMap | O(1) | O(1) | O(1) |
| TreeMap | O(log n) | O(log n) | O(log n) |
| Hashtable | O(1) | O(1) | O(1) |
| ConcurrentHashMap | O(1) | O(1) | O(1) |

---

# HashMap Interview Flow (60-Second Answer)

If asked:

> Explain how HashMap works internally.

You can answer:

> "HashMap stores entries in an array of buckets (`Node<K,V>[]`). When we insert a key-value pair, it calculates the key's `hashCode()`, improves the hash using bit manipulation, computes the bucket index using `(n - 1) & hash`, and stores the entry in that bucket. If another key maps to the same bucket, HashMap handles the collision using separate chaining. In Java 8, if a bucket grows beyond eight nodes and the table has at least 64 buckets, the linked list is converted into a Red-Black Tree to improve worst-case lookup from O(n) to O(log n). HashMap resizes itself when the number of entries exceeds the threshold, which is calculated as `capacity × load factor`."

---

# Chapter Summary

After completing this chapter, you should be able to:

- Explain every major `Map` implementation.
- Describe HashMap internals in detail.
- Explain hashing, buckets, collisions, load factor, capacity, rehashing, and treeification.
- Compare HashMap, LinkedHashMap, TreeMap, Hashtable, and ConcurrentHashMap.
- Recommend the right Map implementation for different production scenarios.
- Answer advanced Java backend interview questions confidently.

---

# Next Chapter

**06-comparable-comparator.md**

Topics:

- Comparable
- Comparator
- compareTo()
- compare()
- Comparable vs Comparator
- Multiple Sorting Strategies
- Lambda Expressions
- Method References
- Real-world Sorting Scenarios
- Interview Questions