# Chapter 3 – Set

> "A Set is a collection that stores unique elements. It is used when duplicate values are not allowed."

---

# What is a Set?

## 🟢 Short Interview Answer (30–60 seconds)

A **Set** is a collection in the Java Collection Framework that stores **unique elements**. Unlike a List, it does not allow duplicate values. Depending on the implementation, it may preserve insertion order, maintain sorted order, or have no ordering at all.

The major implementations are:

- HashSet
- LinkedHashSet
- TreeSet

---

# Why do we need Set?

Suppose you're building a user registration system.

Users:

```
Alice
Bob
Charlie
Alice
Bob
```

Duplicates are not allowed.

A List would store all values.

A Set automatically removes duplicates.

Result

```
Alice
Bob
Charlie
```

---

# Characteristics of Set

| Feature | Supported |
|----------|-----------|
| Duplicate Elements | ❌ No |
| Null Values | Depends on implementation |
| Ordered | Depends on implementation |
| Sorted | TreeSet |
| Fast Lookup | HashSet |

---

# Set Hierarchy

```text
Iterable
      │
 Collection
      │
     Set
 ┌────┼────────────┐
 │    │            │
HashSet LinkedHashSet TreeSet
```

---

# Set Implementations

| Implementation | Ordering | Duplicate | Null | Internal Structure |
|----------------|----------|-----------|------|--------------------|
| HashSet | No | No | One | HashMap |
| LinkedHashSet | Insertion Order | No | One | LinkedHashMap |
| TreeSet | Sorted | No | No | Red-Black Tree |

---

# How Does Set Remove Duplicates?

This is one of the most common interview questions.

Set does **not** compare objects using `==`.

Instead it uses:

```
hashCode()

↓

equals()
```

Flow

```
New Object

↓

hashCode()

↓

Bucket

↓

equals()

↓

Duplicate?

↓

Reject
```

---

# Example

```java
Set<String> technologies = new HashSet<>();

technologies.add("Java");
technologies.add("Spring");
technologies.add("Java");

System.out.println(technologies);
```

Output

```
[Java, Spring]
```

The second `"Java"` is ignored.

---

# Why Doesn't HashSet Store Duplicates?

Internally, `HashSet` uses a `HashMap`.

Each element becomes a **key** in the map.

Example

```java
set.add("Java");
```

Internally

```java
map.put("Java", PRESENT);
```

When `"Java"` is inserted again

```java
map.put("Java", PRESENT);
```

The key already exists.

Therefore the insertion is ignored.

---

# Internal Structure

```
HashSet

↓

HashMap

↓

Bucket

↓

Node

↓

Object
```

The value is a constant dummy object called `PRESENT`.

Simplified source code:

```java
private static final Object PRESENT = new Object();

private transient HashMap<E, Object> map;

public boolean add(E e) {
    return map.put(e, PRESENT) == null;
}
```

This is why `HashSet` inherits the performance characteristics of `HashMap`.

---

# Time Complexity

| Operation | Average | Worst Case |
|------------|---------|------------|
| add() | O(1) | O(n) |
| remove() | O(1) | O(n) |
| contains() | O(1) | O(n) |

> Since Java 8, heavy collisions are optimized using treeification, improving worst-case performance for affected buckets.

---

# Advantages

- Prevents duplicate elements
- Fast insertion
- Fast lookup
- Fast deletion
- Ideal for uniqueness checks

---

# Disadvantages

- No indexing
- No random access
- Ordering depends on implementation
- Cannot retrieve elements by position

---

# Real-world Use Cases

✅ Unique usernames

✅ Unique email addresses

✅ Product IDs

✅ Tags

✅ Permission sets

✅ Cache keys

---

# Production Decision Guide

| Requirement | Collection |
|-------------|------------|
| Unique values | HashSet |
| Unique + insertion order | LinkedHashSet |
| Unique + sorted order | TreeSet |

---

# Interview Questions

### Why doesn't Set allow duplicates?

Because it uses `hashCode()` and `equals()` to determine whether an element already exists.

---

### Can Set contain null?

| Implementation | Null Allowed |
|---------------|--------------|
| HashSet | Yes (one) |
| LinkedHashSet | Yes (one) |
| TreeSet | No (natural ordering) |

---

### Which Set implementation is used most?

**HashSet**, because it provides the best average-case performance for insertion, deletion, and lookup.

---

# Common Interview Mistakes

❌ "Set removes duplicates using ==."

Wrong.

It uses:

- `hashCode()`
- `equals()`

---

❌ "HashSet maintains insertion order."

Wrong.

It provides **no ordering guarantee**.

---

# Key Takeaways

- Set stores unique elements.
- HashSet uses HashMap internally.
- Duplicate detection relies on `hashCode()` and `equals()`.
- Choose the Set implementation based on ordering requirements.

---

# HashSet

## 🟢 Short Interview Answer (30–60 seconds)

`HashSet` is the most commonly used implementation of the `Set` interface. It stores unique elements, provides **O(1)** average time complexity for insertion, deletion, and lookup, and internally uses a **HashMap**.

It does **not** maintain insertion order or sorting.

---

# Internal Working of HashSet

Many developers think HashSet has its own implementation.

It doesn't.

Internally,

```
HashSet

↓

HashMap

↓

Bucket

↓

Node

↓

Object
```

Every element inserted into HashSet becomes a **key** inside HashMap.

Example

```java
HashSet<String> set = new HashSet<>();

set.add("Java");
```

Internally

```java
HashMap<String, Object> map = new HashMap<>();

map.put("Java", PRESENT);
```

Where

```java
private static final Object PRESENT = new Object();
```

---

# Source Code Insight

Simplified JDK Source

```java
public class HashSet<E> {

    private transient HashMap<E,Object> map;

    private static final Object PRESENT = new Object();

    public boolean add(E e){

        return map.put(e,PRESENT)==null;

    }

}
```

Notice

HashSet doesn't store values.

Only keys.

---

# Bucket Structure

Suppose we insert

```
Java

Spring

Docker

Kafka
```

```
Hash Function

↓

Bucket Index

↓

Bucket

↓

Node
```

Example

```
Bucket 0

↓

Java

Bucket 3

↓

Docker

Bucket 7

↓

Spring

↓

Kafka
```

---

# Why is HashSet Fast?

HashMap calculates

```
hashCode()

↓

Bucket

↓

Direct Lookup
```

No traversal of every element.

Average

```
O(1)
```

---

# Characteristics

| Feature | Supported |
|----------|-----------|
| Duplicate | ❌ |
| Ordering | ❌ |
| Sorted | ❌ |
| Null | One |
| Thread Safe | ❌ |

---

# Real-world Use Cases

- Unique Usernames
- Unique Emails
- Cache Keys
- Product IDs
- Coupon Codes
- Permission Lists

---

# LinkedHashSet

## 🟢 Short Interview Answer

`LinkedHashSet` is a HashSet that maintains **insertion order**.

Internally it uses

```
LinkedHashMap
```

instead of

```
HashMap
```

---

# Internal Structure

```
LinkedHashSet

↓

LinkedHashMap

↓

Hash Table

+

Doubly Linked List
```

Diagram

```
Bucket

↓

Java

↓

Spring

↓

Docker

↓

Kafka

Insertion Order

Java

↓

Spring

↓

Docker

↓

Kafka
```

---

# Source Code Insight

```java
LinkedHashMap<E,Object>
```

instead of

```java
HashMap<E,Object>
```

This extra linked list maintains insertion order.

---

# Characteristics

| Feature | Supported |
|----------|-----------|
| Duplicate | ❌ |
| Insertion Order | ✅ |
| Sorted | ❌ |
| Null | One |

---

# Advantages

- Predictable iteration
- Fast lookup
- Unique elements

---

# Disadvantages

- More memory
- Slightly slower than HashSet

---

# Production Use Cases

- API Response Order
- Ordered Cache Keys
- Navigation Menus
- Recently Viewed Items

---

# TreeSet

## 🟢 Short Interview Answer

TreeSet stores unique elements in **sorted order**.

Internally it uses a

```
Red-Black Tree
```

Therefore insertion, deletion, and searching take

```
O(log n)
```

---

# Internal Structure

```
TreeSet

↓

TreeMap

↓

Red Black Tree
```

Unlike HashSet

No buckets.

No hashing.

---

# Red-Black Tree

Example

```
          50
        /    \
      30      70
     /  \    /  \
   20   40 60   80
```

The tree automatically balances itself after every insertion.

This guarantees

```
Height

=

log n
```

instead of

```
n
```

---

# Characteristics

| Feature | Supported |
|----------|-----------|
| Duplicate | ❌ |
| Sorted | ✅ |
| Insertion Order | ❌ |
| Null | ❌ |
| Thread Safe | ❌ |

---

# Why TreeSet doesn't allow null?

TreeSet compares elements.

```
null.compareTo(...)
```

is impossible.

Therefore

```
NullPointerException
```

---

# Source Code Insight

```
TreeSet

↓

TreeMap

↓

Red Black Tree
```

Every element becomes a key in TreeMap.

---

# Complexity

| Operation | Complexity |
|------------|------------|
| add() | O(log n) |
| remove() | O(log n) |
| contains() | O(log n) |

---

# Real-world Use Cases

- Leaderboards
- Rankings
- Sorted Reports
- Date Scheduling
- Student Rankings

---

# HashSet vs LinkedHashSet vs TreeSet

| Feature | HashSet | LinkedHashSet | TreeSet |
|-----------|---------|---------------|----------|
| Duplicate | ❌ | ❌ | ❌ |
| Ordering | None | Insertion | Sorted |
| Internal Structure | HashMap | LinkedHashMap | Red-Black Tree |
| add() | O(1) | O(1) | O(log n) |
| contains() | O(1) | O(1) | O(log n) |
| remove() | O(1) | O(1) | O(log n) |
| Null | One | One | No |

---

# Which Set Should You Choose?

Need

```
Fastest Lookup

↓

HashSet
```

Need

```
Maintain Order

↓

LinkedHashSet
```

Need

```
Sorted Data

↓

TreeSet
```

---

# Production Decision Guide

| Requirement | Recommended Collection |
|--------------|------------------------|
| Remove Duplicates | HashSet |
| Unique + Preserve Order | LinkedHashSet |
| Unique + Sorted | TreeSet |
| Highest Performance | HashSet |
| Reports | TreeSet |

---

# Common Interview Mistakes

❌ "HashSet stores data in an array."

Wrong.

It stores data inside a **HashMap**.

---

❌ "LinkedHashSet is sorted."

Wrong.

It only preserves insertion order.

---

❌ "TreeSet uses HashMap."

Wrong.

It uses

```
TreeMap

↓

Red Black Tree
```

---

# Interview Questions

### Why is HashSet faster than TreeSet?

HashSet uses hashing with average **O(1)** lookup.

TreeSet uses a balanced tree with **O(log n)** operations.

---

### Why doesn't TreeSet allow duplicates?

Because elements are compared during insertion.

If comparison returns **0**, the element is considered a duplicate and is not inserted.

---

### Which Set implementation is used most?

HashSet.

Unless ordering or sorting is specifically required.

---

### Can LinkedHashSet be used as a cache?

Yes.

Because it preserves insertion order and offers fast lookup.

(An LRU cache usually uses `LinkedHashMap`, but `LinkedHashSet` can be useful in ordered-set scenarios.)

---

# How Does HashSet Remove Duplicates?

This is one of the most frequently asked Java Backend interview questions.

Many developers answer:

> "HashSet doesn't allow duplicates."

That is correct but incomplete.

The interviewer actually wants to know **HOW** HashSet determines that an object is a duplicate.

---

# Duplicate Detection Process

When an element is inserted into a HashSet, Java performs two checks.

```
New Object

↓

hashCode()

↓

Find Bucket

↓

equals()

↓

Duplicate?

↓

Reject / Insert
```

Notice that Java does **not** compare every object in the Set.

Instead it performs intelligent bucket-based lookup.

---

# Step 1 : hashCode()

Suppose we insert

```java
set.add("Java");
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

Java converts it into

```
Bucket Number
```

Example

```
Bucket 2
```

---

# Step 2 : Bucket Lookup

Suppose Bucket 2 currently contains

```
Java
Spring
```

Java DOES NOT search every bucket.

It searches ONLY

```
Bucket 2
```

This is why HashSet is fast.

---

# Step 3 : equals()

Suppose Bucket 2 contains

```
Java
```

Now Java calls

```java
existing.equals(newObject)
```

If

```
true
```

Duplicate detected.

Insertion rejected.

If

```
false
```

Object inserted.

---

# Complete Flow

```
Employee

↓

hashCode()

↓

Bucket 5

↓

equals()

↓

Duplicate?

↓

Reject

or

Insert
```

---

# Example

```java
Set<String> technologies = new HashSet<>();

technologies.add("Java");
technologies.add("Spring");
technologies.add("Java");
```

Execution

```
Insert Java

↓

hashCode()

↓

Bucket 4

↓

Empty

↓

Insert

-----------------

Insert Spring

↓

Bucket 7

↓

Insert

-----------------

Insert Java

↓

Bucket 4

↓

equals()

↓

Already Exists

↓

Ignore
```

Output

```
Java

Spring
```

---

# Why hashCode() Alone Is Not Enough?

Suppose

```
Employee A

hashCode()

↓

10

Employee B

hashCode()

↓

10
```

Same hash code.

Different objects.

This is called

```
Hash Collision
```

Therefore Java must call

```
equals()
```

to verify.

---

# Why equals() Alone Is Not Enough?

Suppose

100000 Employees.

If Java compares every employee using equals()

Time

```
O(n)
```

Instead Java first narrows the search using

```
hashCode()

↓

Bucket

↓

equals()
```

Much faster.

---

# Rule

Always remember

```
hashCode()

↓

Find Candidate Bucket

↓

equals()

↓

Confirm Equality
```

Both methods work together.

---

# Interview Question

### Does HashSet use == ?

No.

It uses

```
hashCode()

+

equals()
```

---

# hashCode() Contract

If

```
a.equals(b)
```

returns

```
true
```

then

```
a.hashCode()

==

b.hashCode()
```

must also be true.

---

# equals() Contract

Reflexive

```
a.equals(a)

↓

true
```

---

Symmetric

```
a.equals(b)

↓

b.equals(a)
```

---

Transitive

```
a.equals(b)

↓

true

b.equals(c)

↓

true

↓

a.equals(c)
```

---

Consistent

Multiple calls

↓

Same result

(unless object changes)

---

# Custom Object Example

Without overriding

```java
class Employee{

    int id;

    String name;

}
```

```java
Employee e1 =
new Employee(1,"Alice");

Employee e2 =
new Employee(1,"Alice");
```

```
e1.equals(e2)

↓

false
```

Because

```
Object.equals()

↓

Reference Comparison
```

HashSet stores BOTH objects.

Wrong behavior.

---

# Correct Implementation

```java
@Override
public boolean equals(Object obj){

    if(this==obj)
        return true;

    if(obj==null || getClass()!=obj.getClass())
        return false;

    Employee employee=(Employee)obj;

    return id==employee.id;
}
```

---

```java
@Override
public int hashCode(){

    return Objects.hash(id);

}
```

Now

```
Employee(1)

Employee(1)
```

Duplicate detected.

Only one object stored.

---

# Interview Question

### What happens if only equals() is overridden?

Wrong.

HashSet may still store duplicates because different hash codes place objects into different buckets.

---

### What happens if only hashCode() is overridden?

Wrong.

Objects in the same bucket still require equals() to confirm equality.

---

# Memory Layout

```
HashSet

↓

HashMap

↓

Bucket

↓

Node

↓

Object
```

Objects with the same bucket remain together.

---

# Collision Example

Suppose

```
Java

↓

Bucket 3

Spring

↓

Bucket 3

Docker

↓

Bucket 3
```

```
Bucket 3

↓

Java

↓

Spring

↓

Docker
```

Multiple objects.

Same bucket.

Different objects.

Collision handled correctly.

---

# Common Interview Mistakes

❌ "HashSet compares every object."

Wrong.

Only objects inside the same bucket.

---

❌ "hashCode() determines equality."

Wrong.

hashCode() only finds the bucket.

equals() confirms equality.

---

❌ "equals() alone removes duplicates."

Wrong.

Both methods must work together.

---

# Best Practices

✅ Always override

- equals()
- hashCode()

together.

---

✅ Use immutable fields like

```
id

email

username
```

for equality.

---

❌ Never use mutable fields in hashCode().

Changing them after insertion may make the object unreachable in the HashSet.

---

# Production Example

Suppose your application stores

```
User

↓

email
```

Email is unique.

Implement

```
equals()

↓

email

hashCode()

↓

email
```

Now duplicate registrations are automatically rejected.

---

# Interview Cheat Sheet

| Question | Answer |
|----------|--------|
| How does HashSet remove duplicates? | hashCode() + equals() |
| Does HashSet use == ? | No |
| Why hashCode()? | Find bucket |
| Why equals()? | Confirm equality |
| Override only equals()? | Wrong |
| Override only hashCode()? | Wrong |
| Override both? | Correct |

---

# How Does TreeSet Maintain Sorting?

This is one of the most common Java Backend interview questions.

Many developers answer:

> "TreeSet automatically sorts elements."

Correct.

But the interviewer wants to know **HOW**.

---

# Internal Working

TreeSet internally uses

```
TreeMap

↓

Red-Black Tree
```

It **does not** use hashing.

Every insertion is performed using comparisons.

```
New Element

↓

compareTo()

or

Comparator

↓

Find Position

↓

Insert

↓

Balance Tree
```

---

# Example

```java
TreeSet<Integer> numbers = new TreeSet<>();

numbers.add(50);
numbers.add(20);
numbers.add(70);
numbers.add(10);
numbers.add(90);
```

Internally

```
          50
         /  \
       20    70
      /        \
    10          90
```

Notice

Tree remains sorted.

---

# Why Red-Black Tree?

Imagine a normal Binary Search Tree.

```
10

↓

20

↓

30

↓

40

↓

50
```

Height

```
n
```

Searching

```
O(n)
```

Bad performance.

---

Now Red-Black Tree

```
         30
       /    \
     20      40
    /          \
  10            50
```

Height

```
log n
```

Searching

```
O(log n)
```

Excellent performance.

---

# Red-Black Tree Properties

Interview level explanation

1.

Every node is

```
Red

or

Black
```

---

2.

Root is always

```
Black
```

---

3.

No two consecutive Red nodes.

---

4.

Every path has the same number of Black nodes.

---

These rules automatically keep the tree balanced.

---

# compareTo() vs Comparator

TreeSet requires ordering.

Ordering comes from

```
Comparable

↓

compareTo()
```

or

```
Comparator
```

Example

```java
TreeSet<String> cities = new TreeSet<>();

cities.add("Mumbai");
cities.add("Pune");
cities.add("Delhi");
```

Output

```
Delhi

Mumbai

Pune
```

Alphabetical sorting.

---

# Custom Sorting

```java
TreeSet<Employee> employees =
new TreeSet<>(
Comparator.comparing(Employee::getSalary)
);
```

TreeSet now sorts

```
Salary
```

instead of

```
Name
```

---

# Why Doesn't TreeSet Allow Null?

Interview Question

Suppose

```
TreeSet

↓

null

↓

compareTo()
```

Impossible.

```
NullPointerException
```

Hence

```
Null

↓

Not Allowed
```

---

# Production Decision Guide

## Need

```
Unique Elements

↓

Fastest Performance
```

Choose

```
HashSet
```

---

Need

```
Unique

+

Insertion Order
```

Choose

```
LinkedHashSet
```

---

Need

```
Unique

+

Sorted
```

Choose

```
TreeSet
```

---

# Set Selection Matrix

| Requirement | Collection |
|-------------|------------|
| Fast lookup | HashSet |
| Preserve insertion order | LinkedHashSet |
| Sorted elements | TreeSet |
| Highest performance | HashSet |
| Reports | TreeSet |
| Navigation menus | LinkedHashSet |

---

# Complete Comparison

| Feature | HashSet | LinkedHashSet | TreeSet |
|----------|---------|---------------|----------|
| Duplicate | ❌ | ❌ | ❌ |
| Ordering | None | Insertion | Sorted |
| Internal Structure | HashMap | LinkedHashMap | TreeMap |
| Lookup | O(1) | O(1) | O(log n) |
| Insert | O(1) | O(1) | O(log n) |
| Delete | O(1) | O(1) | O(log n) |
| Null | One | One | ❌ |
| Thread Safe | ❌ | ❌ | ❌ |

---

# Scenario-Based Questions

## Scenario 1

Need unique email IDs.

Choose

```
HashSet
```

---

## Scenario 2

Need recently viewed products in order.

Choose

```
LinkedHashSet
```

---

## Scenario 3

Need leaderboard sorted by score.

Choose

```
TreeSet
```

---

## Scenario 4

Need duplicate-free API response.

Choose

```
HashSet
```

---

## Scenario 5

Need alphabetical country names.

Choose

```
TreeSet
```

---

# 25+ Interview Questions

## 🟢 Beginner

### What is Set?

A collection that stores unique elements.

---

### Does Set allow duplicates?

No.

---

### Does HashSet maintain insertion order?

No.

---

### Does TreeSet maintain sorted order?

Yes.

---

### Which Set implementation is used most?

HashSet.

---

## 🟡 Intermediate

### How does HashSet remove duplicates?

Using

```
hashCode()

↓

equals()
```

---

### Why is HashSet faster?

Hashing provides average O(1) lookup.

---

### Why is TreeSet slower?

Uses a Red-Black Tree.

Operations are O(log n).

---

### Why doesn't TreeSet allow null?

Because comparison with null is not possible.

---

### Difference between HashSet and LinkedHashSet?

HashSet

↓

No ordering.

LinkedHashSet

↓

Insertion order.

---

## 🔴 Advanced

### Explain HashSet internal working.

HashSet internally uses HashMap.

Each element becomes a key.

---

### Explain TreeSet internal working.

TreeSet internally uses TreeMap backed by a Red-Black Tree.

---

### Explain duplicate removal.

hashCode()

↓

Bucket

↓

equals()

↓

Duplicate?

↓

Reject.

---

### Why must equals() and hashCode() be overridden together?

Because HashSet relies on both methods to locate and verify duplicates correctly.

---

### Which Set should be used in production?

Depends.

- HashSet → Performance
- LinkedHashSet → Order
- TreeSet → Sorting

---

# Common Interview Mistakes

❌ HashSet is sorted.

Wrong.

---

❌ LinkedHashSet is sorted.

Wrong.

---

❌ TreeSet uses hashing.

Wrong.

Uses

```
TreeMap

↓

Red-Black Tree
```

---

❌ equals() removes duplicates.

Wrong.

Both

```
hashCode()

+

equals()
```

are required.

---

# One-Day Revision Sheet

## Remember

✅ Set stores unique elements.

✅ HashSet uses HashMap.

✅ LinkedHashSet uses LinkedHashMap.

✅ TreeSet uses TreeMap.

✅ TreeMap uses Red-Black Tree.

---

### Time Complexity

| Collection | add() | contains() | remove() |
|------------|--------|------------|----------|
| HashSet | O(1) | O(1) | O(1) |
| LinkedHashSet | O(1) | O(1) | O(1) |
| TreeSet | O(log n) | O(log n) | O(log n) |

---

# Interview Cheat Sheet

| Requirement | Choose |
|-------------|--------|
| Remove duplicates | HashSet |
| Preserve order | LinkedHashSet |
| Sort data | TreeSet |
| Fast lookup | HashSet |
| Reports | TreeSet |
| Navigation menu | LinkedHashSet |

---

# Chapter Summary

After completing this chapter, you should be able to:

- Explain the Set interface and its characteristics.
- Compare HashSet, LinkedHashSet, and TreeSet.
- Describe how duplicates are removed using `hashCode()` and `equals()`.
- Explain why TreeSet uses a Red-Black Tree.
- Choose the correct Set implementation for different production scenarios.
- Answer common Java backend interview questions related to sets.

---

# Next Chapter

**04-queue.md**

Topics:

- What is Queue?
- Queue Hierarchy
- PriorityQueue
- ArrayDeque
- Deque
- BlockingQueue (Introduction)
- Internal Working
- Producer-Consumer Pattern
- Performance Comparison
- Interview Questions