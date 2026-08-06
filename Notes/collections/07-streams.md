# Chapter 7 – Streams

> "A Stream is a sequence of elements that supports functional-style operations for processing data without modifying the original collection."

---

# What is a Stream?

## 🟢 Short Interview Answer (30–60 seconds)

A **Stream** is a sequence of elements that allows us to perform operations such as filtering, mapping, sorting, grouping, and reducing in a declarative and functional style.

Unlike Collections, Streams **do not store data**.

They process data obtained from a source like:

- List
- Set
- Map
- Arrays
- Files
- Database Results

---

# Why Were Streams Introduced?

Before Java 8

Suppose we want

```
Employees

↓

Salary > 50000

↓

Sort

↓

Collect Names
```

Traditional approach

```
Loop

↓

if

↓

Another Loop

↓

Temporary List

↓

Sorting

↓

Another Loop
```

A lot of boilerplate code.

---

Java 8

```
employees

↓

stream()

↓

filter()

↓

sorted()

↓

map()

↓

collect()
```

Cleaner.

Readable.

Maintainable.

---

# What Problems Do Streams Solve?

Without Streams

- Multiple loops
- Temporary collections
- Complex code
- More bugs

With Streams

- Functional programming
- Cleaner code
- Better readability
- Easy parallel processing

---

# Stream Characteristics

| Feature | Supported |
|----------|-----------|
| Stores Data | ❌ |
| Processes Data | ✅ |
| Lazy Evaluation | ✅ |
| Functional Programming | ✅ |
| Parallel Processing | ✅ |
| Immutable Source | ✅ |

---

# Stream vs Collection

⭐⭐⭐⭐ Frequently Asked

| Collection | Stream |
|------------|---------|
| Stores data | Processes data |
| Can be modified | Read-only view |
| Eager | Lazy |
| Multiple iterations | Single-use |
| Memory based | Pipeline based |

---

# Collection Example

```java
List<String> names = List.of(
        "Java",
        "Spring",
        "Docker"
);
```

Collection

```
Stores Data
```

---

# Stream Example

```java
names.stream()
     .filter(name -> name.startsWith("J"))
     .forEach(System.out::println);
```

Stream

```
Processes Data
```

---

# Stream Pipeline

⭐⭐⭐⭐ Most Important

Every Stream consists of three stages.

```
Source

↓

Intermediate Operations

↓

Terminal Operation
```

---

# Source

Examples

```java
list.stream();

Arrays.stream(array);

Stream.of(1,2,3);
```

---

# Intermediate Operations

Examples

```
filter()

map()

sorted()

distinct()

limit()

skip()

peek()
```

Characteristics

- Lazy
- Returns another Stream
- Can be chained

---

# Terminal Operations

Examples

```
collect()

forEach()

count()

reduce()

findFirst()

findAny()

toList()

min()

max()
```

Characteristics

- Ends the pipeline
- Produces the final result
- Executes all previous operations

---

# Pipeline Example

```java
employees

.stream()

.filter(e -> e.getSalary() > 50000)

.map(Employee::getName)

.sorted()

.toList();
```

Execution

```
Source

↓

filter()

↓

map()

↓

sorted()

↓

toList()
```

---

# Lazy Evaluation

⭐⭐⭐⭐ Very Important

Streams do **not** execute intermediate operations immediately.

Example

```java
List<String> names =
        List.of("Java","Spring");

names.stream()

.filter(System.out::println);
```

Output

```
Nothing
```

Why?

Because there is **no terminal operation**.

---

Now

```java
names.stream()

.filter(name -> {

    System.out.println(name);

    return true;

})

.count();
```

Output

```
Java

Spring
```

The pipeline executes only when a terminal operation is reached.

---

# Single-Use Streams

Interview Question ⭐

```java
Stream<String> stream =
        names.stream();

stream.count();

stream.count();
```

Output

```
IllegalStateException
```

Reason

A Stream can be consumed only once.

To process the data again, create a new Stream.

---

# Stream Lifecycle

```
Collection

↓

stream()

↓

Intermediate Operations

↓

Terminal Operation

↓

Closed
```

After a terminal operation, the Stream cannot be reused.

---

# How Streams Work Internally

```
Collection

↓

Create Stream

↓

Build Pipeline

↓

Terminal Operation

↓

Process Elements

↓

Return Result
```

Notice

Intermediate operations are only **registered**.

They are executed later.

---

# Advantages

- Cleaner code
- Functional programming
- Lazy evaluation
- Easy chaining
- Supports parallel processing
- Less boilerplate

---

# Disadvantages

- Single-use
- Debugging can be harder
- Not ideal for every problem
- May be slower for very small collections due to pipeline overhead

---

# Real-world Backend Use Cases

- Filtering API responses
- Mapping DTOs
- Sorting database results
- Aggregating reports
- Grouping data
- Processing logs
- Analytics

---

# Interview Questions

### What is a Stream?

A Stream is a sequence of elements used for processing data in a functional style without storing it.

---

### Does a Stream store data?

No.

It processes data from another source.

---

### Can Streams modify the original Collection?

No.

Streams do not modify the source collection.

---

### What are the three stages of a Stream?

```
Source

↓

Intermediate Operations

↓

Terminal Operation
```

---

### Why are intermediate operations called lazy?

Because they are not executed until a terminal operation is invoked.

---

### Can a Stream be reused?

No.

A Stream is consumed after a terminal operation.

---

# Common Interview Mistakes

❌ Streams store data.

Wrong.

Collections store data.

Streams process data.

---

❌ filter() executes immediately.

Wrong.

It executes only after a terminal operation.

---

❌ Streams modify collections.

Wrong.

Streams leave the source collection unchanged.

---

# Key Takeaways

- Stream is a data processing pipeline.
- Collections store data; Streams process it.
- Streams are lazy.
- Streams are single-use.
- Every Stream has:
  - Source
  - Intermediate Operations
  - Terminal Operation

---

# Intermediate Stream Operations

⭐⭐⭐⭐⭐ Most Frequently Used in Java Backend

Intermediate operations transform or filter a Stream.

Characteristics:

- Lazy
- Return another Stream
- Can be chained
- Execute only after a terminal operation

---

# filter()

## 🟢 Interview Answer (30–60 seconds)

`filter()` is used to select elements that satisfy a given condition.

It accepts a `Predicate<T>` and returns a new Stream containing only the matching elements.

---

# Syntax

```java
stream.filter(predicate)
```

---

# Example

```java
List<Integer> numbers =
        List.of(10,20,35,40,55);

numbers.stream()

.filter(number -> number > 30)

.forEach(System.out::println);
```

Output

```
35

40

55
```

---

# Internal Working

```
10

↓

Condition?

↓

Rejected

----------------

35

↓

Condition?

↓

Accepted

↓

Next Operation
```

---

# Real-world Example

Filter employees

```
Salary > 50000
```

```java
employees.stream()

.filter(employee -> employee.getSalary() > 50000)

.toList();
```

---

# map()

⭐⭐⭐⭐⭐ One of the Most Asked

## 🟢 Interview Answer

`map()` transforms each element into another object.

Unlike `filter()`, it does **not remove elements**.

Instead, it converts one type into another.

---

# Syntax

```java
stream.map(function)
```

---

# Example

```java
List<String> names =
        List.of("java","spring","docker");

names.stream()

.map(String::toUpperCase)

.forEach(System.out::println);
```

Output

```
JAVA

SPRING

DOCKER
```

---

# Another Example

Employee

↓

Employee Name

```java
employees.stream()

.map(Employee::getName)

.toList();
```

Transformation

```
Employee

↓

String
```

---

# filter() vs map()

| filter() | map() |
|-----------|--------|
| Removes elements | Transforms elements |
| Returns same type | Can return different type |
| Uses Predicate | Uses Function |

---

Example

```
Employee

↓

filter()

↓

Employee

↓

map()

↓

String
```

---

# flatMap()

⭐⭐⭐⭐ Interview Favorite

## Why flatMap()?

Suppose

```java
List<List<String>>
```

Example

```
[
 [Java, Spring],
 [Docker],
 [Kafka, Redis]
]
```

Need

```
Java

Spring

Docker

Kafka

Redis
```

---

Without flatMap()

```
List

↓

List

↓

List
```

---

With flatMap()

```
One Stream
```

---

# Example

```java
List<List<String>> technologies =
List.of(

List.of("Java","Spring"),

List.of("Docker"),

List.of("Kafka","Redis")

);

technologies.stream()

.flatMap(List::stream)

.forEach(System.out::println);
```

Output

```
Java

Spring

Docker

Kafka

Redis
```

---

# Difference

```
map()

↓

Transforms

```

```
flatMap()

↓

Transforms

+

Flattens
```

---

# distinct()

## Interview Answer

Removes duplicate elements.

---

Example

```java
List<Integer> numbers =
List.of(1,2,2,3,3,4,5);

numbers.stream()

.distinct()

.forEach(System.out::println);
```

Output

```
1

2

3

4

5
```

---

Internally

Uses

```
HashSet
```

to track seen elements.

---

# sorted()

Sorts stream elements.

Natural Order

```java
numbers.stream()

.sorted()

.toList();
```

Output

```
1

2

3

4
```

---

Descending

```java
numbers.stream()

.sorted(
Comparator.reverseOrder()
)

.toList();
```

---

Custom Object

```java
employees.stream()

.sorted(

Comparator.comparing(
Employee::getSalary
)

)

.toList();
```

---

# peek()

⭐⭐⭐⭐ Frequently Asked

## What is peek()?

`peek()` performs an action on each element **without changing the Stream**.

Mostly used for

```
Debugging
```

---

Example

```java
numbers.stream()

.peek(System.out::println)

.filter(number -> number > 20)

.count();
```

Output

```
10

20

30

40
```

---

Important

`peek()` should **not** be used for business logic.

Its intended purpose is debugging and tracing.

---

# limit()

Returns only the first

```
N Elements
```

Example

```java
numbers.stream()

.limit(3)

.forEach(System.out::println);
```

Output

```
10

20

30
```

---

# skip()

Skips the first

```
N Elements
```

Example

```java
numbers.stream()

.skip(2)

.forEach(System.out::println);
```

Output

```
30

40

50
```

---

# Chaining Operations

Example

```java
employees.stream()

.filter(employee ->
employee.getSalary() > 50000)

.sorted(

Comparator.comparing(
Employee::getSalary
)

)

.map(Employee::getName)

.limit(5)

.toList();
```

Pipeline

```
Source

↓

filter()

↓

sorted()

↓

map()

↓

limit()

↓

toList()
```

---

# Real-world Backend Examples

## Employee API

```
Employees

↓

Salary > 50000

↓

Sort

↓

Names
```

---

## Product API

```
Products

↓

Category

↓

Sort

↓

Top 10
```

---

## Student Portal

```
Students

↓

Marks > 75

↓

Sort

↓

Names
```

---

# Time Complexity

| Operation | Complexity |
|------------|------------|
| filter() | O(n) |
| map() | O(n) |
| flatMap() | O(n) |
| distinct() | O(n) Average |
| sorted() | O(n log n) |
| limit() | O(1) (short-circuiting) |
| skip() | O(n) |
| peek() | O(n) |

---

# Common Interview Mistakes

❌ filter() changes elements.

Wrong.

It only selects elements.

---

❌ map() removes elements.

Wrong.

It transforms elements.

---

❌ flatMap() only maps.

Wrong.

It maps **and flattens** nested structures.

---

❌ peek() should contain business logic.

Wrong.

It is primarily intended for debugging and observing the pipeline.

---

# Interview Questions

### Difference between filter() and map()?

`filter()` selects elements.

`map()` transforms elements.

---

### Difference between map() and flatMap()?

`map()` creates a one-to-one transformation.

`flatMap()` transforms and flattens nested structures into a single Stream.

---

### Why use distinct()?

To remove duplicate elements.

---

### Which operation is used for debugging?

```
peek()
```

---

### Which operation is used for pagination?

```
skip()

+

limit()
```

---

# Quick Revision

Need

```
Filter Data

↓

filter()
```

Need

```
Transform Data

↓

map()
```

Need

```
Flatten Nested Lists

↓

flatMap()
```

Need

```
Remove Duplicates

↓

distinct()
```

Need

```
Sort

↓

sorted()
```

Need

```
Debug

↓

peek()
```

Need

```
Pagination

↓

skip()

+

limit()
```

---

---

# Terminal Operations

⭐⭐⭐⭐⭐ Most Used in Spring Boot & Backend Development

Terminal operations produce the final result of a Stream.

Characteristics

- Execute the Stream pipeline
- Produce a value or side effect
- Close the Stream
- Cannot be chained after execution

---

# collect()

## 🟢 Interview Answer (30–60 seconds)

`collect()` is the most commonly used terminal operation.

It collects Stream elements into a collection or another result using a `Collector`.

---

# Syntax

```java
stream.collect(Collectors.toList());
```

---

# Example

```java
List<String> names =

employees.stream()

.map(Employee::getName)

.collect(Collectors.toList());
```

Output

```
[Amit, Rahul, Priya]
```

---

# Common Collectors

| Collector | Result |
|------------|--------|
| toList() | List |
| toSet() | Set |
| toMap() | Map |
| joining() | String |
| groupingBy() | Map<Group,List> |
| partitioningBy() | Two Groups |
| counting() | Long |
| mapping() | Transform Inside Group |
| collectingAndThen() | Post Processing |

---

# Collectors.toList()

Collects elements into a List.

```java
List<Integer> numbers =

Stream.of(1,2,3,4)

.collect(Collectors.toList());
```

---

# Collectors.toSet()

Collects unique elements.

```java
Set<Integer> numbers =

Stream.of(1,2,2,3)

.collect(Collectors.toSet());
```

Output

```
1

2

3
```

---

# Collectors.toMap()

Converts Stream into a Map.

Example

```java
Map<Integer,String> map =

employees.stream()

.collect(

Collectors.toMap(

Employee::getId,

Employee::getName

)

);
```

Output

```
101 → Rahul

102 → Amit
```

---

# reduce()

⭐⭐⭐⭐⭐ Interview Favorite

## What is reduce()?

Reduce combines all Stream elements into a single result.

---

Example

```
10

20

30

40

↓

100
```

---

# Syntax

```java
stream.reduce(identity, accumulator)
```

---

# Sum Example

```java
int sum =

numbers.stream()

.reduce(

0,

Integer::sum

);
```

Output

```
100
```

---

# Multiplication

```java
int product =

numbers.stream()

.reduce(

1,

(a,b)->a*b

);
```

---

# Largest Number

```java
Optional<Integer> max =

numbers.stream()

.reduce(Integer::max);
```

---

# Internal Working

```
10

↓

20

↓

30

↓

40

↓

Accumulator

↓

100
```

---

# reduce() Use Cases

- Sum
- Product
- Maximum
- Minimum
- Concatenate Strings
- Aggregate Objects

---

# Collectors.joining()

Used for Strings.

Example

```java
String result =

Stream.of(

"Java",

"Spring",

"Docker"

)

.collect(

Collectors.joining(", ")

);
```

Output

```
Java, Spring, Docker
```

---

# Collectors.counting()

Counts elements.

Example

```java
Long count =

employees.stream()

.collect(

Collectors.counting()

);
```

---

Equivalent

```java
employees.stream()

.count();
```

---

# groupingBy()

⭐⭐⭐⭐⭐ Most Important Collector

Groups objects by a property.

---

Example

Employees

```
IT

HR

IT

Finance
```

Grouping

```
IT

↓

Employee List

HR

↓

Employee List
```

---

Example

```java
Map<String,List<Employee>> employeesByDept =

employees.stream()

.collect(

Collectors.groupingBy(

Employee::getDepartment

)

);
```

---

Output

```
IT

↓

Rahul

Amit

---------------

HR

↓

Priya
```

---

# counting() with groupingBy()

Need

```
Department

↓

Employee Count
```

```java
Map<String,Long> count =

employees.stream()

.collect(

Collectors.groupingBy(

Employee::getDepartment,

Collectors.counting()

)

);
```

Output

```
IT

↓

5

HR

↓

3
```

---

# mapping()

Transforms grouped data.

Example

Need

```
Department

↓

Employee Names
```

Instead of

```
Employee Objects
```

```java
Collectors.groupingBy(

Employee::getDepartment,

Collectors.mapping(

Employee::getName,

Collectors.toList()

)

)
```

Output

```
IT

↓

Rahul

Amit

---------------

HR

↓

Priya
```

---

# partitioningBy()

Interview Favorite

Creates exactly

```
Two Groups
```

Based on

```
true

false
```

---

Example

Need

```
Salary > 50000
```

Output

```
true

↓

Employees

----------------

false

↓

Employees
```

---

Example

```java
Map<Boolean,List<Employee>> result =

employees.stream()

.collect(

Collectors.partitioningBy(

employee ->

employee.getSalary() > 50000

)

);
```

---

Difference

| groupingBy() | partitioningBy() |
|--------------|------------------|
| Multiple Groups | Exactly Two Groups |
| Map<K,List> | Map<Boolean,List> |

---

# collectingAndThen()

Performs one final operation after collecting.

Example

```java
List<String> names =

employees.stream()

.map(Employee::getName)

.collect(

Collectors.collectingAndThen(

Collectors.toList(),

Collections::unmodifiableList

)

);
```

Now

```
Immutable List
```

---

# Real-world Backend Examples

## Employee Service

```
Employees

↓

Department

↓

Grouping
```

↓

```
groupingBy()
```

---

## Dashboard

```
Orders

↓

Count

↓

counting()
```

---

## Product API

```
Products

↓

Category

↓

Price

↓

toMap()
```

---

## Student Result

```
Students

↓

Pass

Fail

↓

partitioningBy()
```

---

## Report Generation

```
Employee Names

↓

CSV

↓

joining()
```

---

# Time Complexity

| Operation | Complexity |
|------------|------------|
| collect() | O(n) |
| reduce() | O(n) |
| groupingBy() | O(n) |
| partitioningBy() | O(n) |
| joining() | O(n) |
| counting() | O(n) |
| toMap() | O(n) |

---

# Common Interview Mistakes

❌ reduce() only calculates sum.

Wrong.

It performs any aggregation.

---

❌ groupingBy() returns List.

Wrong.

Returns

```
Map<Key,List<Value>>
```

---

❌ partitioningBy() creates many groups.

Wrong.

Always

```
Two
```

---

❌ collect() only creates List.

Wrong.

It can create

- List
- Set
- Map
- String
- Custom Results

---

# Interview Questions

### Difference between reduce() and collect()?

| reduce() | collect() |
|-----------|-----------|
| Single Result | Collection or Structured Result |
| Aggregation | Collection Building |
| Immutable Reduction | Mutable Collection Supported |

---

### When should you use groupingBy()?

When grouping objects by a property.

---

### When should you use partitioningBy()?

When dividing data into exactly two groups.

---

### Which collector is used most?

```
Collectors.toList()

Collectors.groupingBy()

Collectors.toMap()
```

---

# Quick Revision

Need

```
List

↓

toList()
```

Need

```
Set

↓

toSet()
```

Need

```
Map

↓

toMap()
```

Need

```
Grouping

↓

groupingBy()
```

Need

```
Two Groups

↓

partitioningBy()
```

Need

```
Aggregation

↓

reduce()
```

Need

```
CSV

↓

joining()
```

Need

```
Immutable Collection

↓

collectingAndThen()
```

---

---

# Finding Elements

Streams provide several methods to retrieve elements without processing the entire collection.

The most commonly used methods are:

- findFirst()
- findAny()

---

# findFirst()

## 🟢 Interview Answer (30–60 seconds)

`findFirst()` returns the **first element** of the Stream.

Return Type

```java
Optional<T>
```

---

# Example

```java
List<String> names =

List.of(

"Java",

"Spring",

"Docker"

);

Optional<String> result =

names.stream()

.findFirst();

System.out.println(result.get());
```

Output

```
Java
```

---

# Characteristics

- Returns Optional
- Preserves encounter order
- Useful for ordered streams

---

# findAny()

## Interview Answer

Returns **any element** from the Stream.

Especially useful with

```
Parallel Streams
```

because Java can return whichever matching element is found first by any thread.

---

# Example

```java
Optional<String> result =

names.parallelStream()

.findAny();
```

Possible Output

```
Java
```

or

```
Spring
```

or

```
Docker
```

---

# findFirst() vs findAny()

| Feature | findFirst() | findAny() |
|----------|-------------|------------|
| Returns | First Element | Any Matching Element |
| Ordered Streams | Yes | May not preserve order |
| Parallel Performance | Slightly Lower | Better |
| Return Type | Optional<T> | Optional<T> |

---

# Match Operations

⭐⭐⭐⭐ Frequently Asked

Java provides

- anyMatch()
- allMatch()
- noneMatch()

---

# anyMatch()

Returns

```
true
```

if **at least one** element matches.

---

Example

```java
boolean result =

numbers.stream()

.anyMatch(

number -> number > 50

);
```

---

# allMatch()

Returns

```
true
```

only if **every** element matches.

---

Example

```java
boolean result =

numbers.stream()

.allMatch(

number -> number > 0

);
```

---

# noneMatch()

Returns

```
true
```

if **no** element matches.

---

Example

```java
boolean result =

numbers.stream()

.noneMatch(

number -> number < 0

);
```

---

# Match Comparison

Suppose

```
10

20

30

40
```

Condition

```
>25
```

Results

```
anyMatch()

↓

true

-----------------

allMatch()

↓

false

-----------------

noneMatch()

↓

false
```

---

# Optional

⭐⭐⭐⭐ Interview Favorite

Streams often return

```
Optional<T>
```

instead of

```
null
```

---

Example

```java
Optional<Employee> employee =

employees.stream()

.findFirst();
```

---

# Why Optional?

Without Optional

```java
Employee employee = findEmployee();

employee.getName();
```

Risk

```
NullPointerException
```

---

With Optional

```java
employee

.ifPresent(

e -> System.out.println(

e.getName()

)

);
```

Safer.

---

# Common Optional Methods

| Method | Purpose |
|----------|---------|
| isPresent() | Check value |
| isEmpty() | No value |
| get() | Retrieve value |
| orElse() | Default value |
| orElseThrow() | Throw exception |
| ifPresent() | Execute if value exists |

---

# Parallel Streams

⭐⭐⭐⭐ Frequently Asked

## What is Parallel Stream?

A Parallel Stream divides work across multiple threads using the

```
ForkJoinPool
```

---

Example

```java
employees

.parallelStream()

.filter(

employee ->

employee.getSalary() > 50000

)

.count();
```

---

# Internal Working

Normal Stream

```
Thread

↓

Process All Elements
```

---

Parallel Stream

```
Thread 1

↓

Elements

------------

Thread 2

↓

Elements

------------

Thread 3

↓

Elements

↓

Combine Results
```

---

# Advantages

- Better performance for large datasets
- Multi-core CPU utilization
- Automatic parallelization

---

# Disadvantages

- Thread creation overhead
- Order may not be preserved
- Not beneficial for small collections
- Shared mutable state can cause bugs

---

# When to Use Parallel Streams?

✅ Large datasets

✅ CPU-intensive operations

✅ Independent computations

---

# When NOT to Use?

❌ Small collections

❌ Database calls

❌ Network requests

❌ Operations requiring strict ordering

---

# Performance Considerations

Sequential Stream

```
One Thread
```

Better for

- Small datasets
- Simple operations

---

Parallel Stream

```
Multiple Threads
```

Better for

- Large datasets
- CPU-intensive processing

---

# Stream Performance Tips

✅ Prefer sequential streams unless profiling shows a benefit.

---

✅ Avoid modifying shared mutable objects inside Stream operations.

---

✅ Prefer method references where appropriate.

---

✅ Use primitive streams (`IntStream`, `LongStream`, `DoubleStream`) for numeric data to avoid boxing overhead.

Example

```java
IntStream.range(1, 100)
         .sum();
```

---

# Real-world Backend Examples

## Search API

```
Employees

↓

filter()

↓

findFirst()
```

---

## Validation

```
Users

↓

allMatch()

↓

Valid?
```

---

## Fraud Detection

```
Transactions

↓

anyMatch()

↓

Suspicious?
```

---

## Student Portal

```
Students

↓

noneMatch()

↓

Failed?
```

---

## Analytics

```
Millions of Records

↓

parallelStream()
```

---

# Time Complexity

| Operation | Complexity |
|------------|------------|
| findFirst() | O(1) Best, O(n) Worst |
| findAny() | O(1) Best, O(n) Worst |
| anyMatch() | O(1) Best, O(n) Worst |
| allMatch() | O(1) Best, O(n) Worst |
| noneMatch() | O(1) Best, O(n) Worst |
| parallelStream() | Depends on workload |

---

# Common Interview Mistakes

❌ Parallel Streams are always faster.

Wrong.

They help only when the workload is large enough to justify parallel execution.

---

❌ findAny() always returns the first element.

Wrong.

It may return any matching element, especially in parallel execution.

---

❌ Optional replaces every null.

Wrong.

Optional is mainly intended as a return type to express the possible absence of a value. It is generally **not recommended** for entity fields or method parameters.

---

❌ Streams are always better than loops.

Wrong.

Simple loops can be clearer and faster for straightforward tasks.

---

# Interview Questions

### Difference between findFirst() and findAny()?

`findFirst()` respects encounter order.

`findAny()` may return any matching element and is optimized for parallel processing.

---

### Difference between anyMatch() and allMatch()?

`anyMatch()` succeeds if at least one element matches.

`allMatch()` requires every element to match.

---

### Why use Optional?

To explicitly represent the presence or absence of a value and reduce accidental `NullPointerException`s.

---

### When should you use Parallel Streams?

For large, CPU-bound workloads with independent operations.

---

# Quick Revision

Need

```
First Element

↓

findFirst()
```

Need

```
Any Element

↓

findAny()
```

Need

```
At Least One

↓

anyMatch()
```

Need

```
All Elements

↓

allMatch()
```

Need

```
No Elements

↓

noneMatch()
```

Need

```
Large CPU Task

↓

parallelStream()
```

Need

```
Safe Return Value

↓

Optional
```

---

---

# Scenario-Based Interview Questions

## Scenario 1

### Requirement

Return names of employees earning more than ₹50,000.

### Solution

```java
employees.stream()
         .filter(employee -> employee.getSalary() > 50000)
         .map(Employee::getName)
         .toList();
```

---

## Scenario 2

### Requirement

Group employees by department.

### Solution

```java
employees.stream()

.collect(

Collectors.groupingBy(
Employee::getDepartment)

);
```

---

## Scenario 3

### Requirement

Count employees in each department.

### Solution

```java
employees.stream()

.collect(

Collectors.groupingBy(

Employee::getDepartment,

Collectors.counting()

)

);
```

---

## Scenario 4

### Requirement

Find the highest-paid employee.

### Solution

```java
employees.stream()

.max(

Comparator.comparing(
Employee::getSalary)

);
```

---

## Scenario 5

### Requirement

Find duplicate elements.

### Solution

```java
Set<Integer> seen = new HashSet<>();

List<Integer> duplicates =

numbers.stream()

.filter(number -> !seen.add(number))

.toList();
```

---

## Scenario 6

### Requirement

Partition employees into

```
Salary > 50000

and

Salary ≤ 50000
```

### Solution

```java
employees.stream()

.collect(

Collectors.partitioningBy(

employee ->

employee.getSalary() > 50000

)

);
```

---

## Scenario 7

### Requirement

Convert List<Employee> into

```
Map<Id, Employee>
```

### Solution

```java
employees.stream()

.collect(

Collectors.toMap(

Employee::getId,

Function.identity()

)

);
```

---

## Scenario 8

### Requirement

Generate

```
Java, Spring, Docker
```

from a list.

### Solution

```java
technologies.stream()

.collect(

Collectors.joining(", ")

);
```

---

## Scenario 9

### Requirement

Flatten

```
List<List<String>>
```

### Solution

```java
lists.stream()

.flatMap(List::stream)

.toList();
```

---

## Scenario 10

### Requirement

Return only the first matching employee.

### Solution

```java
employees.stream()

.filter(employee ->
employee.getSalary() > 50000)

.findFirst();
```

---

# 50+ Interview Questions

## 🟢 Beginner

### What is a Stream?

A Stream is a sequence of elements used to process data functionally without storing it.

---

### Does a Stream store data?

No.

Collections store data.

Streams process data.

---

### Can Streams modify Collections?

No.

The source collection remains unchanged.

---

### What are the three stages of a Stream?

```
Source

↓

Intermediate Operations

↓

Terminal Operation
```

---

### What is lazy evaluation?

Intermediate operations execute only when a terminal operation is invoked.

---

### Can a Stream be reused?

No.

A Stream is consumed after a terminal operation.

---

## 🟡 Intermediate

### Difference between Collection and Stream?

| Collection | Stream |
|------------|---------|
| Stores Data | Processes Data |
| Reusable | Single-use |
| Eager | Lazy |

---

### Difference between filter() and map()?

| filter() | map() |
|-----------|--------|
| Selects elements | Transforms elements |

---

### Difference between map() and flatMap()?

`map()` performs a one-to-one transformation.

`flatMap()` transforms and flattens nested structures.

---

### Difference between collect() and reduce()?

| collect() | reduce() |
|------------|----------|
| Creates collections or structured results | Produces a single aggregated value |

---

### Difference between groupingBy() and partitioningBy()?

| groupingBy() | partitioningBy() |
|--------------|------------------|
| Multiple groups | Exactly two groups |

---

### Difference between findFirst() and findAny()?

`findFirst()` preserves encounter order.

`findAny()` may return any matching element, especially with parallel streams.

---

### Difference between anyMatch(), allMatch(), and noneMatch()?

- `anyMatch()` → at least one matches
- `allMatch()` → every element matches
- `noneMatch()` → no elements match

---

## 🔴 Advanced

### Why are Streams lazy?

To avoid unnecessary computation and enable pipeline optimization.

---

### Why are Streams single-use?

A terminal operation consumes the pipeline and closes the Stream.

---

### Which collector is used most?

```
Collectors.toList()

Collectors.groupingBy()

Collectors.toMap()
```

---

### Which operation removes duplicates?

```
distinct()
```

---

### Which operation transforms objects?

```
map()
```

---

### Which operation filters objects?

```
filter()
```

---

### Which operation performs aggregation?

```
reduce()
```

---

### Which operation groups objects?

```
groupingBy()
```

---

### Which operation partitions data?

```
partitioningBy()
```

---

### Which operation concatenates strings?

```
joining()
```

---

### Which operation is mainly for debugging?

```
peek()
```

---

### When should Parallel Streams be used?

For large, CPU-intensive workloads with independent operations.

---

### When should Parallel Streams be avoided?

- Database calls
- Network requests
- Small datasets
- Order-sensitive operations

---

# Stream Pipeline Visualization

```
Collection

↓

stream()

↓

filter()

↓

map()

↓

sorted()

↓

collect()

↓

List
```

---

# Production Decision Guide

| Requirement | Stream Operation |
|-------------|------------------|
| Filter Data | filter() |
| Transform Data | map() |
| Flatten Nested Lists | flatMap() |
| Remove Duplicates | distinct() |
| Sort Data | sorted() |
| Aggregate Values | reduce() |
| Group Data | groupingBy() |
| Divide Into Two Groups | partitioningBy() |
| Convert to List | toList() / Collectors.toList() |
| Convert to Map | Collectors.toMap() |
| Find First | findFirst() |
| Find Any | findAny() |
| Debug Pipeline | peek() |

---

# Stream Performance Tips

✅ Prefer method references where they improve readability.

---

✅ Use primitive streams

```
IntStream

LongStream

DoubleStream
```

for numeric processing.

---

✅ Avoid unnecessary intermediate operations.

---

✅ Prefer

```java
stream.toList()
```

for an unmodifiable list (Java 16+).

---

✅ Avoid shared mutable state in parallel streams.

---

# Common Interview Mistakes

❌ Streams are always faster.

Wrong.

Performance depends on the workload.

---

❌ Parallel Streams should always be used.

Wrong.

They are beneficial only for suitable CPU-bound tasks.

---

❌ map() filters data.

Wrong.

It transforms data.

---

❌ collect() always returns a List.

Wrong.

It can return

- List
- Set
- Map
- String
- Custom Results

---

❌ peek() should update business objects.

Wrong.

Its intended purpose is observing the pipeline for debugging.

---

# One-Day Revision Sheet

## Remember

✅ Stream

↓

Processes Data

---

✅ Pipeline

```
Source

↓

Intermediate

↓

Terminal
```

---

### Intermediate Operations

- filter()
- map()
- flatMap()
- distinct()
- sorted()
- peek()
- skip()
- limit()

---

### Terminal Operations

- collect()
- reduce()
- count()
- min()
- max()
- forEach()
- findFirst()
- findAny()

---

### Collectors

- toList()
- toSet()
- toMap()
- joining()
- groupingBy()
- partitioningBy()
- counting()
- mapping()
- collectingAndThen()

---

### Match Operations

- anyMatch()
- allMatch()
- noneMatch()

---

### Performance

Sequential

↓

Small datasets

Parallel

↓

Large CPU-intensive datasets

---

# 60-Second Interview Answer

> Java Streams provide a functional approach to processing collections. A Stream does not store data; it processes data from a source such as a Collection or an array. Every Stream consists of a source, intermediate operations, and a terminal operation. Intermediate operations like `filter()`, `map()`, and `sorted()` are lazy and execute only when a terminal operation such as `collect()` or `count()` is invoked. Streams support operations like grouping, partitioning, reducing, and mapping, making code more readable and expressive. They also support parallel processing for suitable workloads, but parallel streams should be used carefully after considering performance characteristics and thread safety.

---

# Chapter Summary

After completing this chapter, you should be able to:

- Explain how Streams work internally.
- Distinguish between intermediate and terminal operations.
- Use filtering, mapping, grouping, partitioning, and reduction effectively.
- Compare `findFirst()` and `findAny()`.
- Explain lazy evaluation and single-use Streams.
- Recommend when to use sequential or parallel streams.
- Answer common Java backend interview questions confidently.

---

# Next Chapter

**08-collections-algorithms.md**

Topics:

- Collections.sort()
- Collections.binarySearch()
- Collections.reverse()
- Collections.shuffle()
- Collections.frequency()
- Collections.min()
- Collections.max()
- Collections.copy()
- Collections.fill()
- Collections.unmodifiableXXX()
- Collections.synchronizedXXX()
- Arrays Utility Methods
- Interview Questions