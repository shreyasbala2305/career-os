# Chapter 2 – List

> "A List is an ordered collection that allows duplicate elements and preserves insertion order."

---

# What is a List?

## 🟢 Short Interview Answer (30–60 seconds)

A **List** is an ordered collection in the Java Collection Framework that stores elements according to their insertion order and allows duplicate values. It provides index-based access, making it suitable when ordering and positional access are important.

The major implementations are:

- ArrayList
- LinkedList
- Vector
- Stack (Legacy)

---

# Why do we need List?

Suppose you're building Netflix.

```
Movie 1
Movie 2
Movie 3
Movie 4
```

The order matters.

If Movie 2 appears before Movie 1 every time, users won't like it.

So we need

- ordered storage
- index access
- duplicates

This is exactly what List provides.

---

# Characteristics of List

| Feature | Supported |
|----------|-----------|
| Ordered | ✅ |
| Duplicates | ✅ |
| Null Values | ✅ |
| Index Based | ✅ |
| Random Access | ArrayList |
| Fast Insert/Delete | LinkedList |

---

# List Hierarchy

```text
Iterable
     │
Collection
     │
    List
 ┌────┼───────────┐
 │    │           │
ArrayList LinkedList Vector
                    │
                  Stack
```

---

# Common Implementations

| Implementation | Internal Structure |
|---------------|--------------------|
| ArrayList | Dynamic Array |
| LinkedList | Doubly Linked List |
| Vector | Dynamic Array |
| Stack | Extends Vector |

---

# ArrayList

## 🟢 Short Interview Answer

ArrayList is the most commonly used implementation of List.

Internally it uses a **dynamic array**.

It provides:

- Fast random access
- Ordered storage
- Duplicate elements
- Automatic resizing

---

# Internal Structure

```
Index

0   1   2   3   4

+---+---+---+---+---+

| A | B | C | D | E |

+---+---+---+---+---+
```

Internally

```java
Object[] elementData;
```

Every element is stored inside this array.

---

# Source Code Insight

From JDK (simplified)

```java
transient Object[] elementData;

private int size;
```

```
elementData

↓

Object[]

↓

Stores every element.
```

size

↓

Current number of elements.

---

# Default Capacity

When you write

```java
List<String> list = new ArrayList<>();
```

Initially

```
Capacity = 0
```

The first insertion creates

```
Capacity = 10
```

---

# Growth Mechanism

Suppose

Capacity

```
10
```

You insert

```
11th element
```

Java creates

```
New Capacity

15
```

Formula

```
Old Capacity

+

Old Capacity / 2
```

Example

```
10

↓

15

↓

22

↓

33

↓

49
```

This avoids frequent copying.

---

# Source Code

Simplified

```java
private Object[] grow(int minCapacity){

    int oldCapacity = elementData.length;

    int newCapacity = oldCapacity + (oldCapacity >> 1);

    return Arrays.copyOf(elementData,newCapacity);

}
```

Notice

```
>>1
```

means

```
Divide by 2
```

So

```
newCapacity

=

1.5 × oldCapacity
```

---

# Adding an Element

```java
list.add("Java");
```

Internally

```
Check Capacity

↓

Enough?

↓

Yes

↓

Insert Element

↓

size++
```

If not enough

```
Grow Array

↓

Copy Old Elements

↓

Insert New Element
```

---

# Complexity

| Operation | Complexity |
|-----------|------------|
| add() at end | O(1) amortized |
| get(index) | O(1) |
| set(index) | O(1) |
| contains() | O(n) |
| remove(index) | O(n) |
| add(index) | O(n) |

---

# Why get() is O(1)?

Because

```
Address

=

Base Address

+

(index × element size)
```

No traversal required.

Direct memory access.

---

# Why insertion in middle is O(n)?

Before

```
A B C D E
```

Insert X after B

```
A B X C D E
```

Java must shift

```
C

↓

D

↓

E
```

Every shifted element takes time.

Hence

```
O(n)
```

---

# Memory Layout

```
ArrayList

↓

Object[]

↓

Reference

↓

Object

↓

Reference

↓

Object
```

Memory is contiguous.

This improves CPU cache locality and makes ArrayList extremely fast for reads.

---

# Advantages

- Fast random access
- Cache friendly
- Low overhead
- Most commonly used List implementation
- Best for read-heavy applications

---

# Disadvantages

- Slow insertion in middle
- Slow deletion in middle
- Array resizing requires copying
- Wastes unused capacity

---

# Production Use Cases

✅ Product catalog

✅ Customer list

✅ Search results

✅ API response list

✅ Employee list

Anywhere frequent reading is more common than insertion/deletion.

---

# Interview Questions

### Why is ArrayList fast?

Because it stores elements in a contiguous dynamic array, enabling direct index-based access.

---

### Why is ArrayList insertion O(n)?

Because elements after the insertion point must be shifted to make room for the new element.

---

### Why does ArrayList grow by 1.5x?

To reduce the number of reallocations while avoiding excessive memory waste.

---

### Can ArrayList store null?

Yes.

It allows multiple null values.

---

# LinkedList

## 🟢 Short Interview Answer (30–60 seconds)

`LinkedList` is a doubly linked list implementation of the `List` interface. Unlike `ArrayList`, it stores elements in nodes connected by references instead of a contiguous array.

It provides:

- Fast insertion
- Fast deletion
- Sequential access
- Bidirectional traversal

---

# Why LinkedList?

Suppose we have

```
A  B  C  D
```

Insert **X** between **B** and **C**

ArrayList

```
A B C D

↓

Shift C

↓

Shift D

↓

Insert X
```

Time

```
O(n)
```

LinkedList

```
A ⇄ B ⇄ C ⇄ D

↓

Change References

↓

A ⇄ B ⇄ X ⇄ C ⇄ D
```

Time

```
O(1)
```

(once the position is reached)

---

# Internal Structure

LinkedList stores every element inside a **Node**.

Each node contains

```
Previous Reference

↓

Current Data

↓

Next Reference
```

Diagram

```
null

↓

[A]

↓

[B]

↓

[C]

↓

[D]

↓

null
```

Actually every node contains

```
null ← A ⇄ B ⇄ C ⇄ D → null
```

---

# Source Code Insight

Simplified JDK Node

```java
private static class Node<E>{

    E item;

    Node<E> next;

    Node<E> prev;

}
```

Every insertion creates a new Node object.

---

# Internal Working

Suppose

```
A ⇄ B ⇄ C ⇄ D
```

Insert X after B

Step 1

```
Create New Node
```

Step 2

```
X.prev = B
```

Step 3

```
X.next = C
```

Step 4

```
B.next = X
```

Step 5

```
C.prev = X
```

Done.

No shifting required.

---

# Complexity

| Operation | Complexity |
|------------|------------|
| addFirst() | O(1) |
| addLast() | O(1) |
| removeFirst() | O(1) |
| removeLast() | O(1) |
| get(index) | O(n) |
| contains() | O(n) |
| insert(index) | O(n) |

---

# Why get(index) is O(n)?

ArrayList

```
Address Calculation

↓

Direct Access
```

LinkedList

```
Head

↓

Node

↓

Node

↓

Node

↓

Target Node
```

Traversal required.

---

# Memory Layout

ArrayList

```
Object[]

↓

Continuous Memory
```

LinkedList

```
Node

↓

Node

↓

Node

↓

Node
```

Nodes are scattered throughout memory.

---

# Advantages

- Fast insertion
- Fast deletion
- No array resizing
- Queue implementation
- Deque implementation

---

# Disadvantages

- Slow random access
- Higher memory usage
- Poor CPU cache locality

---

# Production Use Cases

- Browser history
- Undo / Redo
- Music playlist
- Navigation systems
- Queue implementation

---

# Interview Questions

### Why is LinkedList slower than ArrayList for reading?

Because every access requires node traversal.

---

### Does LinkedList use a singly linked list?

No.

Java uses a **Doubly Linked List**.

---

### Can LinkedList implement Queue?

Yes.

It implements both

- List
- Deque

---

# Vector

## 🟢 Short Interview Answer

`Vector` is a synchronized dynamic array introduced before the Collection Framework.

It is similar to `ArrayList`, but every public method is synchronized.

---

# Internal Structure

```
Object[]
```

Same as ArrayList.

Difference

```
Synchronized Methods
```

---

# Source Code Insight

```java
public synchronized boolean add(E e)
```

Notice

```
synchronized
```

Every operation acquires a lock.

---

# Capacity Growth

ArrayList

```
1.5x
```

Vector

```
2x
```

unless capacityIncrement is specified.

---

# Advantages

- Thread-safe
- Legacy compatibility

---

# Disadvantages

- Slower
- Lock overhead
- Rarely used today

---

# ArrayList vs Vector

| Feature | ArrayList | Vector |
|-----------|-----------|---------|
| Thread Safe | ❌ | ✅ |
| Performance | Faster | Slower |
| Growth | 1.5x | 2x |
| Legacy | No | Yes |

---

# Production Recommendation

Use

```
ArrayList
```

instead of

```
Vector
```

unless maintaining legacy code.

---

# Stack

## 🟢 Short Interview Answer

`Stack` is a legacy LIFO (Last-In First-Out) collection that extends `Vector`.

Although still available, modern Java recommends using `Deque` implementations like `ArrayDeque`.

---

# LIFO

```
Push

↓

Push

↓

Push

↓

Pop

↑
```

Last inserted element comes out first.

---

# Internal Working

Stack extends

```
Vector

↓

Object[]
```

Methods

```
push()

pop()

peek()

empty()

search()
```

---

# Example

```java
Stack<String> stack = new Stack<>();

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

# Why Stack is Legacy?

Problems

- Extends Vector
- Synchronized
- Slower
- Poor design compared to Deque

---

# Modern Alternative

```
Deque<String>

↓

ArrayDeque
```

instead of

```
Stack
```

---

# Stack vs Deque

| Feature | Stack | Deque |
|-----------|--------|--------|
| Legacy | Yes | No |
| Faster | ❌ | ✅ |
| Thread Safe | Yes | No |
| Recommended | ❌ | ✅ |

---

# ArrayList vs LinkedList

| Feature | ArrayList | LinkedList |
|-----------|-----------|------------|
| Internal Structure | Dynamic Array | Doubly Linked List |
| Read | ⭐⭐⭐⭐⭐ | ⭐⭐ |
| Insert Middle | ⭐⭐ | ⭐⭐⭐⭐ |
| Delete Middle | ⭐⭐ | ⭐⭐⭐⭐ |
| Memory Usage | Low | High |
| Cache Friendly | Yes | No |

---

# Production Decision Guide

| Requirement | Best Choice |
|--------------|-------------|
| Fast Random Access | ArrayList |
| Frequent Insert/Delete | LinkedList |
| Thread-safe Legacy | Vector |
| LIFO Stack | ArrayDeque |
| Queue | LinkedList / ArrayDeque |

---

# Common Interview Mistakes

❌ "LinkedList is always faster."

Wrong.

Only insertion/deletion is faster.

Reading is slower.

---

❌ "Stack should always be used."

Wrong.

Prefer

```
ArrayDeque
```

---

# Follow-up Questions

- Why is ArrayList cache-friendly?
- Why does LinkedList consume more memory?
- Why is Vector slower?
- Why does Stack extend Vector?
- Why is ArrayDeque preferred over Stack?

---

# Time Complexity Analysis

Understanding time complexity is crucial because choosing the wrong collection can significantly impact application performance.

---

## ArrayList Time Complexity

| Operation | Complexity | Reason |
|------------|------------|--------|
| add(E) | O(1) (Amortized) | Add at end |
| add(index, E) | O(n) | Elements need shifting |
| get(index) | O(1) | Direct index access |
| set(index, E) | O(1) | Direct replacement |
| remove(index) | O(n) | Elements shift left |
| contains(Object) | O(n) | Linear search |
| iterator() | O(1) | Creates iterator |

---

## LinkedList Time Complexity

| Operation | Complexity | Reason |
|------------|------------|--------|
| addFirst() | O(1) | Update head |
| addLast() | O(1) | Update tail |
| removeFirst() | O(1) | Update head |
| removeLast() | O(1) | Update tail |
| get(index) | O(n) | Node traversal |
| contains(Object) | O(n) | Linear search |
| add(index, E) | O(n) | Traverse then insert |
| remove(index) | O(n) | Traverse then remove |

---

# Visual Complexity Comparison

| Operation | ArrayList | LinkedList |
|------------|-----------|------------|
| Read | ⭐⭐⭐⭐⭐ | ⭐⭐ |
| Insert Beginning | ⭐⭐ | ⭐⭐⭐⭐⭐ |
| Insert Middle | ⭐⭐ | ⭐⭐⭐⭐ |
| Insert End | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| Delete Beginning | ⭐⭐ | ⭐⭐⭐⭐⭐ |
| Delete Middle | ⭐⭐ | ⭐⭐⭐⭐ |
| Delete End | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| Memory Usage | ⭐⭐⭐⭐⭐ | ⭐⭐ |

---

# Memory Comparison

## ArrayList

```text
Object[]

↓

Reference

↓

Object

↓

Reference

↓

Object
```

Memory Usage

```
Low
```

Reason

Only stores references.

---

## LinkedList

```text
Node

↓

Previous

↓

Data

↓

Next
```

Each node stores

- Data
- Previous Pointer
- Next Pointer

Memory Usage

```
High
```

---

# CPU Cache Performance

This is an advanced interview topic.

## ArrayList

```
Memory

↓

Continuous

↓

CPU Cache Friendly
```

Reading multiple elements is very fast because they are stored close together.

---

## LinkedList

```
Memory

↓

Scattered

↓

Cache Misses
```

CPU must jump between different memory locations.

Result

```
Slower Traversal
```

---

# Source Code Walkthrough

## ArrayList.add()

Simplified JDK source

```java
public boolean add(E e) {

    ensureCapacityInternal(size + 1);

    elementData[size++] = e;

    return true;
}
```

### Step 1

Check capacity.

### Step 2

Resize if necessary.

### Step 3

Insert element.

### Step 4

Increment size.

---

## LinkedList.add()

Simplified

```java
Node<E> newNode =
        new Node<>(last, element, null);

last.next = newNode;

last = newNode;
```

No shifting.

Only pointer updates.

---

# Dynamic Array Resizing

Current Capacity

```
10
```

Insert

```
11th Element
```

Java creates

```
15
```

Copies

```
10 Existing Elements
```

Adds

```
11th Element
```

---

# Why doesn't ArrayList grow by one element?

Imagine adding one million elements.

Growing by one each time would require one million copies.

Growing by **1.5x** dramatically reduces the number of resize operations.

---

# Production Decision Guide

## Scenario 1

Requirement

```
Frequently read data

Rare updates
```

Choose

```
ArrayList
```

Reason

Fast random access.

---

## Scenario 2

Requirement

```
Insert frequently
```

Choose

```
LinkedList
```

---

## Scenario 3

Requirement

```
Legacy synchronized code
```

Choose

```
Vector
```

---

## Scenario 4

Requirement

```
LIFO
```

Choose

```
ArrayDeque
```

instead of Stack.

---

## Scenario 5

Requirement

```
API Response

10000 Objects
```

Choose

```
ArrayList
```

Reading dominates.

---

# Real-world Examples

## Shopping Cart

```
ArrayList
```

Reason

Display items repeatedly.

---

## Browser History

```
LinkedList
```

Reason

Frequent insertion and deletion.

---

## Undo / Redo

```
ArrayDeque
```

Reason

Fast stack operations.

---

## Playlist

```
LinkedList
```

Reason

Frequent insertions between songs.

---

## Employee Report

```
ArrayList
```

Reason

Sequential reading.

---

# Best Practices

✅ Program to interfaces.

```java
List<Employee> employees =
        new ArrayList<>();
```

Not

```java
ArrayList<Employee> employees =
        new ArrayList<>();
```

---

✅ Prefer ArrayList unless you have a strong reason to use LinkedList.

---

✅ Prefer ArrayDeque over Stack.

---

✅ Avoid Vector in modern applications.

---

✅ Use Collections.unmodifiableList() or List.copyOf() when returning immutable lists.

---

# Common Mistakes

## Mistake 1

❌ Using LinkedList for random access.

Every get(index) is O(n).

---

## Mistake 2

❌ Using Vector in new applications.

Prefer modern concurrent collections if thread safety is required.

---

## Mistake 3

❌ Assuming LinkedList is always faster.

Insertion is faster.

Reading is slower.

---

## Mistake 4

❌ Using Stack.

Prefer

```
ArrayDeque
```

---

## Production Interview Decision Matrix

| Requirement | Collection |
|-------------|------------|
| Random Access | ArrayList |
| Frequent Insert/Delete | LinkedList |
| Thread-safe Legacy | Vector |
| Stack | ArrayDeque |
| Queue | LinkedList |
| Read-heavy Workload | ArrayList |
| Memory Efficient | ArrayList |

---

# Interview Tips

### If interviewer asks

> Which List implementation should you choose?

Answer

"It depends on the access pattern.

If random access is frequent, I choose ArrayList because it provides O(1) indexed access.

If insertion and deletion dominate and random access is less important, I choose LinkedList.

For stack operations, I prefer ArrayDeque over Stack because it is faster and not a legacy class."

---

# Chapter Summary

- ArrayList uses a dynamic array.
- LinkedList uses a doubly linked list.
- Vector is synchronized and considered legacy.
- Stack extends Vector and should generally be replaced by ArrayDeque.
- ArrayList is preferred for read-heavy workloads.
- LinkedList is useful when insertions and deletions are frequent.
- Understanding memory layout and time complexity helps in selecting the correct implementation.

---

# Frequently Asked Interview Questions

## 🟢 Beginner Level

### Q1. What is a List?

**Answer**

A `List` is an ordered collection in Java that maintains insertion order, allows duplicate elements, supports index-based access, and permits multiple `null` values.

---

### Q2. Which classes implement List?

- ArrayList
- LinkedList
- Vector
- Stack (Legacy)

---

### Q3. Does List allow duplicate elements?

Yes.

```java
List<String> list = new ArrayList<>();

list.add("Java");
list.add("Java");
list.add("Java");
```

Output

```
Java
Java
Java
```

---

### Q4. Does List allow null values?

Yes.

```java
list.add(null);
list.add(null);
```

---

### Q5. Which List implementation is used most?

**ArrayList**

Because most applications perform significantly more reads than insertions or deletions.

---

# 🟡 Intermediate Level

---

### Q6. ArrayList vs LinkedList?

| Feature | ArrayList | LinkedList |
|----------|-----------|------------|
| Internal Structure | Dynamic Array | Doubly Linked List |
| Random Access | O(1) | O(n) |
| Insert/Delete Middle | O(n) | O(n) traversal + O(1) link update |
| Memory | Lower | Higher |

---

### Q7. Why is ArrayList faster?

Because it stores elements in contiguous memory, allowing direct index-based access and better CPU cache utilization.

---

### Q8. Why is LinkedList slower for searching?

Because it must traverse nodes sequentially from the head or tail.

---

### Q9. Why does ArrayList insertion become O(n)?

Elements after the insertion point must be shifted.

```
Before

A B C D E

Insert X

A B X C D E

Shift

C

↓

D

↓

E
```

---

### Q10. Why does LinkedList use more memory?

Each node stores:

- Data
- Previous reference
- Next reference

This adds memory overhead compared to `ArrayList`.

---

### Q11. Why is ArrayList cache friendly?

Because elements are stored in contiguous memory locations.

---

### Q12. Why is LinkedList not cache friendly?

Nodes are scattered across memory, causing cache misses during traversal.

---

# 🔴 Advanced Level

---

### Q13. How does ArrayList grow internally?

Default capacity after the first insertion is typically 10.

When full, it grows by approximately **1.5×**.

```
10

↓

15

↓

22

↓

33
```

---

### Q14. Why doesn't ArrayList grow by one element?

Growing by one would require frequent array copying.

Increasing capacity by 1.5× balances memory usage and performance.

---

### Q15. Explain ArrayList internal implementation.

Internally:

```java
Object[] elementData;
```

Elements are stored inside a dynamically resized array.

---

### Q16. Explain LinkedList internal implementation.

Internally:

```java
Node<E>{

    E item;

    Node<E> next;

    Node<E> prev;

}
```

Each node stores references to both the previous and next nodes.

---

### Q17. Why does LinkedList implement Deque?

Because it supports efficient insertion and removal at both the beginning and end of the list.

---

### Q18. Why is Stack considered legacy?

Because it extends `Vector`, which synchronizes every method.

`ArrayDeque` provides the same functionality with better performance.

---

### Q19. Why is Vector slower?

Every public method is synchronized, introducing locking overhead.

---

### Q20. Which collection should you use for a stack?

Preferred:

```java
Deque<String> stack = new ArrayDeque<>();
```

Not:

```java
Stack<String> stack = new Stack<>();
```

---

# Scenario-Based Interview Questions

---

### Scenario 1

**Requirement**

Store products displayed on an e-commerce website.

**Answer**

`ArrayList`

Reason:

Frequent reads and random access.

---

### Scenario 2

**Requirement**

Browser history.

**Answer**

`LinkedList` or `ArrayDeque`

Reason:

Frequent insertions/removals at the ends.

---

### Scenario 3

**Requirement**

Undo / Redo.

**Answer**

`ArrayDeque`

Reason:

Efficient stack operations.

---

### Scenario 4

**Requirement**

Playlist where songs are frequently inserted between existing songs.

**Answer**

`LinkedList`

---

### Scenario 5

**Requirement**

Display search results from a database.

**Answer**

`ArrayList`

---

### Scenario 6

**Requirement**

Legacy thread-safe application.

**Answer**

`Vector`

(Modern applications should prefer concurrent collections where appropriate.)

---

# Decision Tree

```
Need ordering?

↓

YES

↓

Need random access?

↓

YES

↓

ArrayList

↓

NO

↓

Frequent insertion/deletion?

↓

YES

↓

LinkedList

↓

NO

↓

ArrayList
```

---

# Production Decision Guide

| Requirement | Recommended Collection |
|--------------|------------------------|
| Fast Random Access | ArrayList |
| Frequent Insert/Delete | LinkedList |
| Stack | ArrayDeque |
| Queue | ArrayDeque / LinkedList |
| Legacy Thread-safe List | Vector |
| API Response | ArrayList |
| Search Results | ArrayList |
| Undo / Redo | ArrayDeque |

---

# One-Day Revision Sheet

## Remember

✅ List preserves insertion order.

✅ List allows duplicates.

✅ List allows null values.

---

### ArrayList

- Dynamic Array
- O(1) get()
- O(n) insert/remove (middle)
- Cache friendly

---

### LinkedList

- Doubly Linked List
- O(n) random access
- Better for insert/delete after reaching the node
- Higher memory usage

---

### Vector

- Legacy
- Thread-safe
- Slower than ArrayList

---

### Stack

- Legacy
- Extends Vector
- Prefer `ArrayDeque`

---

# Common Mistakes

❌ Saying LinkedList is always faster.

Only certain operations benefit.

---

❌ Using Stack in new projects.

Prefer `ArrayDeque`.

---

❌ Using LinkedList for frequent random access.

Use `ArrayList` instead.

---

❌ Assuming Vector is recommended for thread safety.

Modern concurrent collections are generally a better choice.

---

# Quick Revision Table

| Topic | Key Point |
|---------|-----------|
| List | Ordered, allows duplicates |
| ArrayList | Dynamic array |
| LinkedList | Doubly linked list |
| Vector | Synchronized legacy list |
| Stack | Legacy LIFO implementation |
| Best Read Performance | ArrayList |
| Best Insert/Delete | LinkedList (after locating the position) |
| Modern Stack | ArrayDeque |

---

# Chapter Summary

After studying this chapter, you should be able to:

- Explain the `List` interface and its characteristics.
- Compare `ArrayList`, `LinkedList`, `Vector`, and `Stack`.
- Describe their internal implementations.
- Analyze time and space complexity.
- Recommend the appropriate implementation for different production scenarios.
- Answer common Java backend interview questions related to lists.

---

# Next Chapter

**03-set.md**

Topics:

- What is a Set?
- HashSet
- LinkedHashSet
- TreeSet
- Duplicate Handling
- Ordering
- Internal Working
- TreeSet (Red-Black Tree)
- Performance Comparison
- Production Use Cases
- Interview Questions
- Decision Guide