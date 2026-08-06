# Chapter 8 – Collections Algorithms

> "The Collections class is a utility class that provides static methods to perform common operations such as sorting, searching, reversing, shuffling, copying, and synchronizing collections."

---

# What is the Collections Class?

## 🟢 Short Interview Answer (30–60 seconds)

`Collections` is a **utility class** in the `java.util` package.

It contains **static methods** for performing common algorithms and operations on Java Collections.

Examples include:

- Sorting
- Searching
- Reversing
- Shuffling
- Finding min/max
- Synchronization
- Creating unmodifiable collections

Unlike the `Collection` interface, the `Collections` class cannot be instantiated.

---

# Collection vs Collections

⭐⭐⭐⭐ Frequently Asked

| Collection | Collections |
|------------|-------------|
| Interface | Utility Class |
| Stores elements | Provides utility methods |
| Implemented by List, Set, Queue | Contains static helper methods |
| Object | Final utility class |

---

# Why Do We Need Collections?

Suppose we have

```java
List<Integer> numbers =
List.of(5,2,8,1);
```

Need

```
Sort

Reverse

Search

Shuffle
```

Instead of writing algorithms manually,

Java provides

```
Collections
```

---

# Common Methods

| Method | Purpose |
|----------|----------|
| sort() | Sorting |
| binarySearch() | Searching |
| reverse() | Reverse Order |
| shuffle() | Random Order |
| swap() | Exchange Elements |
| rotate() | Circular Rotation |
| fill() | Replace All Elements |
| copy() | Copy Collection |
| min() | Minimum Element |
| max() | Maximum Element |
| frequency() | Count Occurrences |
| disjoint() | Check Common Elements |
| replaceAll() | Replace Values |

---

# Collections.sort()

⭐⭐⭐⭐⭐ Most Asked

## What is sort()?

Sorts elements into

```
Ascending Order
```

using natural ordering or a Comparator.

---

# Syntax

```java
Collections.sort(list);
```

---

# Example

```java
List<Integer> numbers =
new ArrayList<>(

List.of(5,2,8,1)

);

Collections.sort(numbers);

System.out.println(numbers);
```

Output

```
[1,2,5,8]
```

---

# Internal Working

Modern JDK

```
Collections.sort()

↓

List.sort()

↓

TimSort
```

TimSort is:

- Stable
- O(n log n)
- Optimized for partially sorted data

---

# Custom Sorting

Using Comparator

```java
Collections.sort(

employees,

Comparator.comparing(
Employee::getSalary)

);
```

---

Descending

```java
Collections.sort(

employees,

Comparator.comparing(
Employee::getSalary)

.reversed()

);
```

---

# Time Complexity

| Case | Complexity |
|-------|------------|
| Best | O(n) |
| Average | O(n log n) |
| Worst | O(n log n) |

---

# Collections.reverseOrder()

Creates a Comparator for

```
Descending Order
```

---

Example

```java
Collections.sort(

numbers,

Collections.reverseOrder()

);
```

Output

```
9

7

5

3

1
```

---

# Collections.binarySearch()

⭐⭐⭐⭐ Frequently Asked

## What is binarySearch()?

Searches an element in a

```
Sorted Collection
```

using the Binary Search algorithm.

---

# Requirement

The list **must already be sorted**.

---

# Syntax

```java
Collections.binarySearch(

list,

key

);
```

---

# Example

```java
List<Integer> numbers =

List.of(

1,3,5,7,9

);

int index =

Collections.binarySearch(

numbers,

7

);

System.out.println(index);
```

Output

```
3
```

---

# Internal Working

```
Middle Element

↓

Less?

↓

Search Left

------------

Greater?

↓

Search Right
```

---

# Time Complexity

| Operation | Complexity |
|------------|------------|
| Binary Search | O(log n) |

---

# Important Interview Question

### Can binarySearch() work on an unsorted List?

No.

The collection **must be sorted according to the same ordering used for searching**.

Otherwise, the result is undefined.

---

# Real-world Backend Examples

## Employee API

Need

```
Sort Employees

↓

Salary
```

↓

```
Collections.sort()
```

---

## Product Catalog

Need

```
Highest Price First
```

↓

```
reverseOrder()
```

---

## Student Search

Need

```
Roll Number Lookup
```

↓

```
binarySearch()
```

---

# Common Interview Mistakes

❌ Collections is the same as Collection.

Wrong.

Collection is an interface.

Collections is a utility class.

---

❌ binarySearch() sorts the list.

Wrong.

The list must already be sorted.

---

❌ reverseOrder() reverses the list.

Wrong.

It creates a Comparator for descending sorting.

---

# Interview Questions

### What is Collections?

A utility class containing static methods for operating on collections.

---

### Why is Collections a utility class?

Because it provides reusable algorithms without maintaining state.

---

### Which algorithm does Collections.sort() use?

TimSort (through `List.sort()` in modern JDKs).

---

### What is the complexity of binarySearch()?

```
O(log n)
```

---

### When should binarySearch() be used?

Only on collections sorted according to the same comparator or natural ordering used for the search.

---

# Quick Revision

Need

```
Ascending Sort

↓

Collections.sort()
```

Need

```
Descending Sort

↓

Collections.reverseOrder()
```

Need

```
Fast Search

↓

Collections.binarySearch()
```

---

---

# Collections.reverse()

## 🟢 Interview Answer (30–60 seconds)

`Collections.reverse()` reverses the order of elements in a List.

Unlike `reverseOrder()`, it **does not sort** the list.

It simply reverses the current order.

---

# Syntax

```java
Collections.reverse(list);
```

---

# Example

```java
List<Integer> numbers =
new ArrayList<>(

List.of(1,2,3,4,5)

);

Collections.reverse(numbers);

System.out.println(numbers);
```

Output

```
[5,4,3,2,1]
```

---

# Difference

Original

```
1

2

3

4

5
```

After

```
Collections.reverse()
```

```
5

4

3

2

1
```

---

# Collections.reverse() vs reverseOrder()

| reverse() | reverseOrder() |
|------------|----------------|
| Reverses current order | Creates descending Comparator |
| Modifies List | Used during sorting |

---

# Collections.shuffle()

## What is shuffle()?

Randomly rearranges the elements of a list.

Useful for:

- Random quizzes
- Games
- Randomized testing
- Load balancing

---

# Syntax

```java
Collections.shuffle(list);
```

---

# Example

```java
List<Integer> numbers =
new ArrayList<>(

List.of(1,2,3,4,5)

);

Collections.shuffle(numbers);

System.out.println(numbers);
```

Possible Output

```
[3,1,5,2,4]
```

Another execution

```
[5,2,4,1,3]
```

Every execution may produce a different order.

---

# Internal Working

Uses a randomization algorithm (Fisher–Yates shuffle in OpenJDK).

Time Complexity

```
O(n)
```

---

# Collections.swap()

Swaps two elements in a list.

---

# Syntax

```java
Collections.swap(

list,

index1,

index2

);
```

---

# Example

```java
List<String> names =
new ArrayList<>(

List.of(

"Java",

"Spring",

"Docker"

)

);

Collections.swap(

names,

0,

2

);

System.out.println(names);
```

Output

```
Docker

Spring

Java
```

---

# Collections.rotate()

⭐⭐⭐ Frequently Asked

Rotates elements by a specified distance.

Positive distance

↓

Rotate Right

Negative distance

↓

Rotate Left

---

# Syntax

```java
Collections.rotate(

list,

distance

);
```

---

# Example

```java
List<Integer> numbers =
new ArrayList<>(

List.of(

1,2,3,4,5

)

);

Collections.rotate(

numbers,

2

);

System.out.println(numbers);
```

Output

```
[4,5,1,2,3]
```

---

Rotate Left

```java
Collections.rotate(

numbers,

-2

);
```

Output

```
[3,4,5,1,2]
```

---

# Real-world Example

Task Scheduler

```
Round Robin

↓

Rotate Queue
```

---

# Collections.fill()

Replaces every element in the list with the same value.

---

# Syntax

```java
Collections.fill(

list,

value

);
```

---

# Example

```java
List<Integer> numbers =
new ArrayList<>(

List.of(

1,2,3,4

)

);

Collections.fill(

numbers,

0

);

System.out.println(numbers);
```

Output

```
[0,0,0,0]
```

---

# Use Cases

- Reset game board
- Initialize arrays
- Testing

---

# Collections.copy()

Copies elements from one list into another.

---

# Syntax

```java
Collections.copy(

destination,

source

);
```

---

# Important Rule

The destination list **must already contain at least as many elements as the source list**.

---

# Example

```java
List<String> source =
List.of(

"Java",

"Spring",

"Docker"

);

List<String> destination =
new ArrayList<>(

Arrays.asList(

"",

"",

""

)

);

Collections.copy(

destination,

source

);

System.out.println(destination);
```

Output

```
Java

Spring

Docker
```

---

# Common Mistake

```java
List<String> destination =
new ArrayList<>();

Collections.copy(

destination,

source

);
```

Output

```
IndexOutOfBoundsException
```

Reason

Destination list has no elements.

---

# Time Complexity

| Method | Complexity |
|----------|------------|
| reverse() | O(n) |
| shuffle() | O(n) |
| swap() | O(1) |
| rotate() | O(n) |
| fill() | O(n) |
| copy() | O(n) |

---

# Real-world Backend Examples

## Quiz Application

Need

```
Random Questions
```

↓

```
shuffle()
```

---

## Gaming

Need

```
Reverse Leaderboard
```

↓

```
reverse()
```

---

## Scheduler

Need

```
Rotate Tasks
```

↓

```
rotate()
```

---

## Data Reset

Need

```
Reset Values
```

↓

```
fill()
```

---

## Duplicate Configuration

Need

```
Copy Defaults
```

↓

```
copy()
```

---

# Common Interview Mistakes

❌ reverse() sorts in descending order.

Wrong.

It simply reverses the current order.

---

❌ shuffle() always gives the same order.

Wrong.

It randomizes the list.

---

❌ copy() creates a new list.

Wrong.

It copies into an existing destination list.

---

❌ rotate() only rotates right.

Wrong.

Positive values rotate right.

Negative values rotate left.

---

# Interview Questions

### Difference between reverse() and reverseOrder()?

`reverse()` changes the current order of elements.

`reverseOrder()` returns a Comparator used for descending sorting.

---

### Which method randomizes elements?

```
Collections.shuffle()
```

---

### Which method exchanges two elements?

```
Collections.swap()
```

---

### Which method replaces all values?

```
Collections.fill()
```

---

### What is the requirement for Collections.copy()?

The destination list must already have enough capacity **and size** to hold all source elements.

---

# Quick Revision

Need

```
Reverse List

↓

reverse()
```

Need

```
Random Order

↓

shuffle()
```

Need

```
Exchange Elements

↓

swap()
```

Need

```
Circular Rotation

↓

rotate()
```

Need

```
Replace All Values

↓

fill()
```

Need

```
Copy List

↓

copy()
```

---

---

# Collections.min()

## 🟢 Interview Answer (30–60 seconds)

`Collections.min()` returns the smallest element in a collection according to its natural ordering or a provided Comparator.

---

# Syntax

```java
Collections.min(collection);
```

---

# Example

```java
List<Integer> numbers =
List.of(10,5,25,3,18);

int min = Collections.min(numbers);

System.out.println(min);
```

Output

```
3
```

---

# Using Comparator

```java
Employee youngest =

Collections.min(

employees,

Comparator.comparing(Employee::getAge)

);
```

---

# Time Complexity

```
O(n)
```

Every element must be examined.

---

# Collections.max()

Returns the largest element.

---

# Syntax

```java
Collections.max(collection);
```

---

# Example

```java
List<Integer> numbers =
List.of(10,5,25,3,18);

int max = Collections.max(numbers);

System.out.println(max);
```

Output

```
25
```

---

# Custom Comparator

```java
Employee highestSalary =

Collections.max(

employees,

Comparator.comparing(
Employee::getSalary)

);
```

---

# Collections.frequency()

⭐⭐⭐ Frequently Asked

Counts how many times an element appears.

---

# Syntax

```java
Collections.frequency(

collection,

element

);
```

---

# Example

```java
List<String> languages =
List.of(

"Java",

"Spring",

"Java",

"Docker",

"Java"

);

int count =

Collections.frequency(

languages,

"Java"

);

System.out.println(count);
```

Output

```
3
```

---

# Internal Working

Checks every element.

```
O(n)
```

---

# Production Example

Need

```
Count

↓

Occurrences

↓

Status

↓

Role

↓

Category
```

---

# Collections.disjoint()

Checks whether two collections have

```
No Common Elements
```

---

# Syntax

```java
Collections.disjoint(

collection1,

collection2

);
```

---

# Example

```java
List<Integer> first =
List.of(1,2,3);

List<Integer> second =
List.of(4,5,6);

boolean result =

Collections.disjoint(

first,

second

);

System.out.println(result);
```

Output

```
true
```

---

Another Example

```java
List<Integer> first =
List.of(1,2,3);

List<Integer> second =
List.of(3,4,5);
```

Output

```
false
```

Because

```
3
```

is common.

---

# Use Cases

- Permission checks
- Role comparison
- Tag matching
- Feature flags

---

# Collections.replaceAll()

Replaces all occurrences of one value with another.

---

# Syntax

```java
Collections.replaceAll(

list,

oldValue,

newValue

);
```

---

# Example

```java
List<String> names =
new ArrayList<>(

List.of(

"Java",

"Spring",

"Java"

)

);

Collections.replaceAll(

names,

"Java",

"Kotlin"

);

System.out.println(names);
```

Output

```
Kotlin

Spring

Kotlin
```

---

# Collections.indexOfSubList()

⭐⭐⭐ Rare but Good Interview Topic

Returns the starting index of the first occurrence of a sublist.

---

# Example

```java
List<Integer> main =
List.of(

1,2,3,4,5,6

);

List<Integer> sub =
List.of(

3,4

);

int index =

Collections.indexOfSubList(

main,

sub

);

System.out.println(index);
```

Output

```
2
```

---

# Collections.lastIndexOfSubList()

Returns the starting index of the last occurrence.

---

# Example

```java
List<Integer> main =
List.of(

1,2,3,4,3,4,5

);

List<Integer> sub =
List.of(

3,4

);

int index =

Collections.lastIndexOfSubList(

main,

sub

);

System.out.println(index);
```

Output

```
4
```

---

# Time Complexity

| Method | Complexity |
|----------|------------|
| min() | O(n) |
| max() | O(n) |
| frequency() | O(n) |
| disjoint() | O(n) Average* |
| replaceAll() | O(n) |
| indexOfSubList() | O(n × m) |
| lastIndexOfSubList() | O(n × m) |

\*Actual performance depends on the collection types used. For example, if one collection is a `HashSet`, membership checks are typically O(1) on average.

---

# Real-world Backend Examples

## Employee Service

Need

```
Highest Salary
```

↓

```
Collections.max()
```

---

## Analytics

Need

```
Most Frequent Status
```

↓

```
frequency()
```

---

## Authorization

Need

```
Common Roles
```

↓

```
disjoint()
```

---

## Migration

Need

```
Replace Old Code

↓

New Code
```

↓

```
replaceAll()
```

---

# Common Interview Mistakes

❌ min() sorts the collection.

Wrong.

It scans to find the minimum.

---

❌ frequency() is O(1).

Wrong.

It checks every element.

---

❌ disjoint() checks equality.

Wrong.

It checks whether the collections share **any** common elements.

---

❌ replaceAll() creates a new list.

Wrong.

It modifies the existing list.

---

# Interview Questions

### Difference between min() and sort()?

`min()` returns only the smallest element.

`sort()` orders the entire collection.

---

### Which method counts occurrences?

```
Collections.frequency()
```

---

### Which method checks whether collections have no common elements?

```
Collections.disjoint()
```

---

### Which method replaces every occurrence?

```
Collections.replaceAll()
```

---

### Which method finds a sublist?

```
indexOfSubList()

lastIndexOfSubList()
```

---

# Quick Revision

Need

```
Minimum

↓

min()
```

Need

```
Maximum

↓

max()
```

Need

```
Occurrences

↓

frequency()
```

Need

```
No Common Elements

↓

disjoint()
```

Need

```
Replace Values

↓

replaceAll()
```

Need

```
Find Sublist

↓

indexOfSubList()
```

Need

```
Find Last Sublist

↓

lastIndexOfSubList()
```

---

---

# Unmodifiable Collections

⭐⭐⭐⭐ Frequently Asked

## What are Unmodifiable Collections?

Unmodifiable collections are **read-only views** of existing collections.

They allow reading data but prevent modifications.

---

# Why Use Unmodifiable Collections?

Suppose

```
Configuration Data

↓

Should Never Change
```

Instead of exposing the original collection,

return an unmodifiable view.

This prevents accidental modifications.

---

# Collections.unmodifiableList()

## Syntax

```java
List<String> list =
new ArrayList<>(

List.of(

"Java",

"Spring",

"Docker"

)

);

List<String> unmodifiable =

Collections.unmodifiableList(list);
```

---

# Example

```java
unmodifiable.add("Kafka");
```

Output

```
UnsupportedOperationException
```

---

# Important Note

The wrapper is **unmodifiable**, but the original list can still change.

```java
list.add("Kafka");

System.out.println(unmodifiable);
```

Output

```
Java

Spring

Docker

Kafka
```

The view reflects changes made to the original list.

---

# Collections.unmodifiableSet()

Creates a read-only Set.

```java
Set<String> set =

Collections.unmodifiableSet(

new HashSet<>(

Set.of("Java","Spring")

)

);
```

---

# Collections.unmodifiableMap()

Creates a read-only Map.

```java
Map<Integer,String> map =

Collections.unmodifiableMap(

Map.of(

1,"Java",

2,"Spring"

)

);
```

---

# Synchronized Collections

⭐⭐⭐⭐ Frequently Asked

## Why?

Collections such as

```
ArrayList

HashMap

HashSet
```

are **not thread-safe**.

Java provides synchronized wrappers.

---

# Collections.synchronizedList()

## Syntax

```java
List<Integer> list =

Collections.synchronizedList(

new ArrayList<>()

);
```

---

# Internal Working

Every method is synchronized.

```
Thread 1

↓

Lock

↓

Execute

↓

Unlock

↓

Thread 2
```

---

# Synchronized Set

```java
Set<String> set =

Collections.synchronizedSet(

new HashSet<>()

);
```

---

# Synchronized Map

```java
Map<Integer,String> map =

Collections.synchronizedMap(

new HashMap<>()

);
```

---

# Iterating Synchronized Collections

Interview Favorite ⭐

This is **not enough**

```java
for(String value : list){

}
```

Correct approach

```java
synchronized(list){

    for(String value : list){

        System.out.println(value);

    }

}
```

Reason

Iteration itself is **not automatically synchronized**.

---

# Unmodifiable vs Synchronized

| Unmodifiable | Synchronized |
|---------------|--------------|
| Read-only | Thread-safe |
| Prevents modification | Prevents concurrent corruption |
| Not thread-safe | Thread-safe |

---

# Arrays Utility Class

Do not confuse

```
Array

Arrays

Collections
```

---

# Arrays.sort()

Sorts arrays.

```java
int[] numbers =

{5,2,8,1};

Arrays.sort(numbers);
```

Output

```
1

2

5

8
```

---

# Arrays.binarySearch()

Searches a sorted array.

```java
int index =

Arrays.binarySearch(

numbers,

5

);
```

Time Complexity

```
O(log n)
```

---

# Arrays.copyOf()

Copies an array.

```java
int[] copy =

Arrays.copyOf(

numbers,

numbers.length

);
```

---

# Arrays.equals()

Checks whether two arrays contain the same elements in the same order.

```java
Arrays.equals(array1,array2);
```

---

# Arrays.deepEquals()

Used for multidimensional arrays.

```java
Arrays.deepEquals(matrix1,matrix2);
```

---

# Arrays.fill()

Replaces every element.

```java
Arrays.fill(

numbers,

0

);
```

Output

```
0

0

0

0
```

---

# Arrays.toString()

Converts an array into a readable String.

```java
System.out.println(

Arrays.toString(numbers)

);
```

Output

```
[1, 2, 3, 4]
```

---

# Arrays.asList()

Converts an array into a fixed-size List.

```java
List<String> list =

Arrays.asList(

"Java",

"Spring",

"Docker"

);
```

---

# Important Interview Question

```java
list.add("Kafka");
```

Output

```
UnsupportedOperationException
```

Reason

`Arrays.asList()` returns a **fixed-size list**, not a regular `ArrayList`.

You can modify existing elements with `set()`, but you cannot change the size.

---

# Time Complexity

| Method | Complexity |
|----------|------------|
| Arrays.sort() | O(n log n) |
| Arrays.binarySearch() | O(log n) |
| Arrays.copyOf() | O(n) |
| Arrays.equals() | O(n) |
| Arrays.deepEquals() | O(n) |
| Arrays.fill() | O(n) |
| Arrays.toString() | O(n) |

---

# Real-world Backend Examples

## Configuration

```
Read-only Settings

↓

unmodifiableList()
```

---

## Shared Cache

```
Multiple Threads

↓

synchronizedMap()
```

---

## Search

```
Sorted Array

↓

binarySearch()
```

---

## Testing

```
Initialize Arrays

↓

fill()
```

---

# Common Interview Mistakes

❌ unmodifiableList() creates an immutable copy.

Wrong.

It creates a **read-only view** of the original collection.

---

❌ synchronizedList() makes iteration automatically thread-safe.

Wrong.

External synchronization is required during iteration.

---

❌ Arrays.asList() returns an ArrayList.

Wrong.

It returns a fixed-size list backed by the array.

---

❌ Arrays.equals() works correctly for nested arrays.

Wrong.

Use

```
Arrays.deepEquals()
```

for multidimensional arrays.

---

# Interview Questions

### Difference between unmodifiableList() and synchronizedList()?

`unmodifiableList()` prevents modifications.

`synchronizedList()` provides thread safety.

---

### Why does Arrays.asList().add() throw an exception?

Because the returned list has a fixed size.

---

### Which method compares multidimensional arrays?

```
Arrays.deepEquals()
```

---

### Which method converts an array into a String?

```
Arrays.toString()
```

---

# Quick Revision

Need

```
Read-only Collection

↓

unmodifiableList()
```

Need

```
Thread Safety

↓

synchronizedList()
```

Need

```
Sort Array

↓

Arrays.sort()
```

Need

```
Search Array

↓

Arrays.binarySearch()
```

Need

```
Copy Array

↓

Arrays.copyOf()
```

Need

```
Compare Arrays

↓

Arrays.equals()

Arrays.deepEquals()
```

Need

```
Fill Array

↓

Arrays.fill()
```

---

---

# Scenario-Based Interview Questions

## Scenario 1

### Requirement

Sort employees by salary.

### Solution

```java
Collections.sort(

employees,

Comparator.comparing(
Employee::getSalary)

);
```

---

## Scenario 2

### Requirement

Sort products in descending price order.

### Solution

```java
Collections.sort(

products,

Comparator.comparing(
Product::getPrice)

.reversed()

);
```

---

## Scenario 3

### Requirement

Randomize quiz questions.

### Solution

```java
Collections.shuffle(

questions

);
```

---

## Scenario 4

### Requirement

Reverse a list.

### Solution

```java
Collections.reverse(

numbers

);
```

---

## Scenario 5

### Requirement

Find the highest-paid employee.

### Solution

```java
Employee employee =

Collections.max(

employees,

Comparator.comparing(
Employee::getSalary)

);
```

---

## Scenario 6

### Requirement

Find the minimum roll number.

### Solution

```java
int minimum =

Collections.min(

rollNumbers

);
```

---

## Scenario 7

### Requirement

Count occurrences of "Java".

### Solution

```java
Collections.frequency(

languages,

"Java"

);
```

---

## Scenario 8

### Requirement

Check whether two users share any common roles.

### Solution

```java
Collections.disjoint(

rolesUserA,

rolesUserB

);
```

If the result is

```
false
```

they share at least one role.

---

## Scenario 9

### Requirement

Expose configuration as read-only.

### Solution

```java
Collections.unmodifiableList(

configuration

);
```

---

## Scenario 10

### Requirement

Create a thread-safe list.

### Solution

```java
Collections.synchronizedList(

new ArrayList<>()

);
```

---

# 40+ Interview Questions

## 🟢 Beginner

### What is the Collections class?

A utility class containing static methods that operate on collections.

---

### Collection vs Collections?

| Collection | Collections |
|------------|-------------|
| Interface | Utility Class |
| Stores data | Provides algorithms |

---

### Which package contains Collections?

```
java.util
```

---

### Which method sorts a list?

```java
Collections.sort()
```

---

### Which method performs binary search?

```java
Collections.binarySearch()
```

---

### Which method reverses a list?

```java
Collections.reverse()
```

---

### Which method randomizes a list?

```java
Collections.shuffle()
```

---

## 🟡 Intermediate

### Difference between reverse() and reverseOrder()?

`reverse()` changes the current order of the list.

`reverseOrder()` returns a Comparator for descending sorting.

---

### Difference between min() and sort()?

`min()` finds only the smallest element.

`sort()` orders the entire collection.

---

### Which method counts duplicate values?

```java
Collections.frequency()
```

---

### Which method checks whether collections share no common elements?

```java
Collections.disjoint()
```

---

### Which method replaces all matching values?

```java
Collections.replaceAll()
```

---

### Which method rotates elements?

```java
Collections.rotate()
```

---

### Which method swaps two elements?

```java
Collections.swap()
```

---

### Which method copies one list into another?

```java
Collections.copy()
```

---

## 🔴 Advanced

### Which sorting algorithm is used by Collections.sort()?

TimSort (through `List.sort()` in modern JDKs).

---

### What is the complexity of Collections.sort()?

```
O(n log n)
```

Average and worst case.

---

### What is the complexity of binarySearch()?

```
O(log n)
```

---

### Can binarySearch() work on an unsorted list?

No.

The list must already be sorted according to the same ordering.

---

### Why does Collections.copy() throw IndexOutOfBoundsException?

Because the destination list must already contain at least as many elements as the source list.

---

### Why does unmodifiableList() still reflect changes?

Because it returns a **read-only view** backed by the original collection.

---

### Why is synchronizedList() not enough during iteration?

Because iteration requires external synchronization.

---

### Why does Arrays.asList().add() fail?

Because it returns a fixed-size list.

---

# Decision Matrix

| Requirement | Method |
|-------------|--------|
| Sort | sort() |
| Descending Sort | reverseOrder() |
| Reverse Current Order | reverse() |
| Search | binarySearch() |
| Randomize | shuffle() |
| Swap | swap() |
| Rotate | rotate() |
| Replace All | fill() |
| Copy | copy() |
| Minimum | min() |
| Maximum | max() |
| Count Occurrences | frequency() |
| Check No Common Elements | disjoint() |
| Replace Values | replaceAll() |
| Read-only Collection | unmodifiableList() |
| Thread-safe Collection | synchronizedList() |

---

# Production Decision Guide

| Requirement | Recommended Method |
|-------------|--------------------|
| Sort API Response | sort() |
| Leaderboard (Descending) | reverseOrder() |
| Random Quiz Questions | shuffle() |
| Rotate Scheduler Tasks | rotate() |
| Reset Cache Values | fill() |
| Duplicate Configuration | copy() |
| Highest Salary | max() |
| Lowest Price | min() |
| Count Status Values | frequency() |
| Role Comparison | disjoint() |
| Immutable Configuration | unmodifiableList() |
| Shared Thread-safe List | synchronizedList() |

---

# Common Interview Mistakes

❌ `Collections.sort()` returns a new list.

Wrong.

It sorts the existing list in place.

---

❌ `reverseOrder()` reverses an existing list.

Wrong.

It creates a Comparator used during sorting.

---

❌ `Collections.copy()` increases the destination list size.

Wrong.

It replaces existing elements only.

---

❌ `unmodifiableList()` creates an independent immutable copy.

Wrong.

It creates a read-only view backed by the original collection.

---

❌ `Arrays.asList()` returns a normal `ArrayList`.

Wrong.

It returns a fixed-size list backed by the array.

---

# One-Day Revision Sheet

## Collections Algorithms

### Sorting

- `sort()`
- `reverseOrder()`

---

### Searching

- `binarySearch()`

---

### Rearranging

- `reverse()`
- `shuffle()`
- `swap()`
- `rotate()`

---

### Modifying

- `fill()`
- `copy()`
- `replaceAll()`

---

### Statistics

- `min()`
- `max()`
- `frequency()`

---

### Comparison

- `disjoint()`
- `indexOfSubList()`
- `lastIndexOfSubList()`

---

### Read-only Wrappers

- `unmodifiableList()`
- `unmodifiableSet()`
- `unmodifiableMap()`

---

### Thread-safe Wrappers

- `synchronizedList()`
- `synchronizedSet()`
- `synchronizedMap()`

---

### Arrays Utility

- `Arrays.sort()`
- `Arrays.binarySearch()`
- `Arrays.copyOf()`
- `Arrays.equals()`
- `Arrays.deepEquals()`
- `Arrays.fill()`
- `Arrays.toString()`
- `Arrays.asList()`

---

# 60-Second Interview Answer

> The `Collections` class is a utility class in the `java.util` package that provides static methods for operating on collections. It includes algorithms such as sorting, searching, reversing, shuffling, rotating, copying, and finding minimum or maximum values. It also provides wrappers for creating synchronized and unmodifiable collections. Commonly used methods include `sort()`, `binarySearch()`, `reverse()`, `shuffle()`, `min()`, `max()`, `frequency()`, `unmodifiableList()`, and `synchronizedList()`. These methods simplify collection manipulation and are widely used in Java backend applications.

---

# Chapter Summary

After completing this chapter, you should be able to:

- Explain the purpose of the `Collections` utility class.
- Use sorting, searching, reversing, and shuffling methods.
- Find minimum, maximum, and frequency values.
- Work with read-only and synchronized collection wrappers.
- Use common `Arrays` utility methods.
- Recommend the appropriate utility method for different production scenarios.
- Answer common Java backend interview questions confidently.

---

<!-- # Next Chapter

**09-coding-patterns.md**

Topics:

- Two Sum
- Contains Duplicate
- Valid Anagram
- Remove Duplicates
- Group Anagrams
- Frequency Counter
- Top K Frequent Elements
- Employee Grouping
- Highest Salary
- Department Count
- Custom Object Sorting
- LRU Cache
- Session Storage
- Leaderboard
- Inventory Management
- 50+ Coding & Scenario Questions -->