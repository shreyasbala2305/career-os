# Chapter 6 – Comparable & Comparator

> "Comparable and Comparator are two mechanisms in Java used to define the ordering of objects. Comparable defines the natural ordering of a class, while Comparator provides custom sorting strategies."

---

# Why Do We Need Sorting?

Suppose we have

Employees

```
Rahul

Salary = 50000

----------------

Amit

Salary = 70000

----------------

Priya

Salary = 60000
```

Now interviewer asks

```
Sort By Salary
```

Tomorrow requirement changes

```
Sort By Name
```

Next

```
Sort By Experience
```

How do we achieve this?

Java provides

```
Comparable

Comparator
```

---

# What is Comparable?

## 🟢 Short Interview Answer (30–60 seconds)

Comparable is an interface in Java used to define the **natural ordering** of objects.

It contains one method

```java
compareTo()
```

implemented inside the class itself.

---

# Interface Definition

```java
public interface Comparable<T>{

    int compareTo(T o);

}
```

Only one method.

```
compareTo()
```

---

# What is Natural Ordering?

Natural ordering means

```
One Default Ordering
```

Examples

```
String

↓

Alphabetical

Integer

↓

Ascending

Date

↓

Chronological
```

For Employee

You decide

```
Employee ID

or

Name

or

Salary
```

Only **one** can be the natural ordering.

---

# Example

```java
public class Employee
implements Comparable<Employee>{

    private int salary;

    @Override
    public int compareTo(Employee other){

        return Integer.compare(
                this.salary,
                other.salary);

    }

}
```

Sorting

```java
Collections.sort(employeeList);
```

Output

```
Ascending Salary
```

---

# compareTo() Return Values

| Return Value | Meaning |
|--------------|---------|
| Negative | Current object is smaller |
| Zero | Equal |
| Positive | Current object is greater |

---

# Example

```java
Integer a = 10;
Integer b = 20;

System.out.println(a.compareTo(b));
```

Output

```
-1
```

Meaning

```
10

<

20
```

---

# Internal Working

Collections.sort()

↓

Calls

```
compareTo()
```

↓

Receives

```
Negative

Zero

Positive
```

↓

Swaps Elements

↓

Sorted List

---

# Example

Before

```
50000

70000

30000
```

compareTo()

↓

Sorted

```
30000

50000

70000
```

---

# Advantages

- Simple
- Built into the class
- Easy to use
- Best for default sorting

---

# Disadvantages

- Only one sorting strategy
- Must modify source code
- Cannot create multiple orderings

---

# Real-world Example

Employee

Natural Order

```
Employee ID
```

Every employee automatically follows this order.

---

# Interview Questions

### What is Comparable?

Interface used to define the natural ordering of objects.

---

### How many methods?

One

```
compareTo()
```

---

### Where is Comparable implemented?

Inside the class whose objects need sorting.

---

### Can Comparable define multiple sorting strategies?

No.

Only one natural ordering.

---

# Common Interview Mistakes

❌ Comparable belongs to Collections.

Wrong.

Comparable belongs to

```
java.lang
```

---

❌ compareTo() returns boolean.

Wrong.

Returns

```
int
```

---

# Comparator

## 🟢 Short Interview Answer

Comparator is an interface used to define **custom sorting logic** outside the class.

Unlike Comparable, it allows multiple sorting strategies without modifying the original class.

---

# Interface Definition

```java
public interface Comparator<T>{

    int compare(T o1,T o2);

}
```

Method

```
compare()
```

---

# Why Comparator?

Suppose Employee already implements Comparable

```
Sort By Salary
```

Tomorrow requirement

```
Sort By Name
```

Tomorrow

```
Sort By Experience
```

Tomorrow

```
Sort By Department
```

Comparable cannot handle all these.

Comparator can.

---

# Example

```java
Comparator<Employee> salaryComparator =
Comparator.comparing(Employee::getSalary);
```

Sort

```java
Collections.sort(
        employees,
        salaryComparator
);
```

---

# Another Example

```java
Comparator<Employee> nameComparator =
Comparator.comparing(Employee::getName);
```

Same Employee class.

Different sorting.

---

# compare()

Return Values

| Return | Meaning |
|----------|---------|
| Negative | First object smaller |
| Zero | Equal |
| Positive | First object greater |

---

# Internal Working

Collections.sort()

↓

Comparator.compare()

↓

Negative?

↓

Swap

↓

Continue

---

# Lambda Example

```java
employees.sort(

(a,b)->

Integer.compare(

a.getSalary(),

b.getSalary()

)

);
```

No separate comparator class required.

---

# Method Reference Example

```java
employees.sort(

Comparator.comparing(

Employee::getName

)

);
```

Cleaner.

---

# Advantages

- Multiple sorting strategies
- No modification of model class
- Reusable
- Lambda friendly

---

# Disadvantages

- Slightly more code
- External sorting logic

---

# Production Use Cases

Sort Employees

↓

Salary

Sort Employees

↓

Department

Sort Employees

↓

Joining Date

Sort Products

↓

Price

Sort Students

↓

Marks

---

# Interview Questions

### What is Comparator?

An interface used for custom sorting.

---

### Where is Comparator implemented?

Outside the model class.

---

### How many methods?

One

```
compare()
```

---

### Can Comparator provide multiple sorting strategies?

Yes.

Unlimited.

---

# Quick Comparison

| Feature | Comparable | Comparator |
|----------|------------|------------|
| Package | java.lang | java.util |
| Method | compareTo() | compare() |
| Location | Inside Class | Outside Class |
| Sorting | Natural | Custom |
| Strategies | One | Multiple |
| Lambda Support | No | Yes |

---

# Common Interview Mistakes

❌ Comparator modifies the object.

Wrong.

It only defines comparison logic.

---

❌ Comparable is used for multiple sorting strategies.

Wrong.

Use Comparator for multiple sorting requirements.

---

# Advanced Comparator Features

Java 8 introduced powerful utility methods in the `Comparator` interface that make sorting cleaner, more expressive, and easier to maintain.

Instead of writing large comparator classes, we can compose comparators using helper methods.

---

# Comparator.comparing()

## 🟢 Interview Answer

`Comparator.comparing()` creates a Comparator based on a property of an object.

Instead of manually comparing fields, Java extracts the field and performs the comparison.

---

## Traditional Way

```java
Comparator<Employee> salaryComparator =
new Comparator<Employee>(){

    @Override
    public int compare(Employee e1, Employee e2){

        return Integer.compare(
                e1.getSalary(),
                e2.getSalary()
        );
    }
};
```

---

## Modern Java 8+

```java
Comparator<Employee> salaryComparator =
Comparator.comparing(Employee::getSalary);
```

Much shorter and easier to read.

---

# Sorting Example

```java
employees.sort(
        Comparator.comparing(Employee::getSalary)
);
```

Output

```
30000

45000

70000

90000
```

---

# Comparator.reversed()

Sometimes we need descending order.

Instead of

```
30000

45000

70000
```

we need

```
70000

45000

30000
```

---

Example

```java
employees.sort(

Comparator

.comparing(Employee::getSalary)

.reversed()

);
```

Output

```
90000

70000

45000

30000
```

---

# thenComparing()

⭐⭐⭐⭐ Frequently Asked

Suppose two employees have the same salary.

Sort by

```
Salary

↓

Name
```

---

Example

```java
employees.sort(

Comparator

.comparing(Employee::getSalary)

.thenComparing(Employee::getName)

);
```

Execution

```
Salary

↓

If Equal

↓

Name
```

---

Example

Input

```
Rahul

60000

--------------

Amit

60000

--------------

Priya

50000
```

Output

```
Priya

50000

--------------

Amit

60000

--------------

Rahul

60000
```

---

# Multiple Sorting

Need

```
Department

↓

Salary

↓

Name
```

Easy.

```java
employees.sort(

Comparator

.comparing(Employee::getDepartment)

.thenComparing(Employee::getSalary)

.thenComparing(Employee::getName)

);
```

Unlimited chaining.

---

# nullsFirst()

Interview Question ⭐

Suppose

```
Java

Spring

null

Docker
```

Sorting throws

```
NullPointerException
```

Solution

```java
Comparator.nullsFirst(
Comparator.naturalOrder()
)
```

Example

```java
employees.sort(

Comparator.comparing(

Employee::getName,

Comparator.nullsFirst(
Comparator.naturalOrder()
)

)

);
```

Output

```
null

Docker

Java

Spring
```

---

# nullsLast()

Instead of

```
null

Java

Spring
```

Need

```
Java

Spring

null
```

Example

```java
employees.sort(

Comparator.comparing(

Employee::getName,

Comparator.nullsLast(
Comparator.naturalOrder()
)

)

);
```

---

# Comparator.naturalOrder()

Sort

```
Ascending
```

```java
Comparator.naturalOrder()
```

Example

```
10

20

30

40
```

---

# Comparator.reverseOrder()

Sort

```
Descending
```

```java
Comparator.reverseOrder()
```

Output

```
40

30

20

10
```

---

# Stream Sorting

Comparator is commonly used with Streams.

Example

```java
employees

.stream()

.sorted(

Comparator.comparing(
Employee::getSalary
)

)

.toList();
```

---

Descending

```java
employees

.stream()

.sorted(

Comparator

.comparing(Employee::getSalary)

.reversed()

)

.toList();
```

---

# Real-world Examples

## Employee Service

Requirement

```
Sort

↓

Salary

↓

Descending
```

```java
Comparator.comparing(

Employee::getSalary

).reversed()
```

---

## Product API

Requirement

```
Category

↓

Price

↓

Name
```

```java
Comparator

.comparing(Product::getCategory)

.thenComparing(Product::getPrice)

.thenComparing(Product::getName);
```

---

## Student Result

Requirement

```
Marks

↓

Name
```

```java
Comparator

.comparing(Student::getMarks)

.reversed()

.thenComparing(Student::getName);
```

---

# Comparator Utility Methods

| Method | Purpose |
|----------|---------|
| comparing() | Compare using a property |
| thenComparing() | Multi-level sorting |
| reversed() | Descending order |
| naturalOrder() | Ascending order |
| reverseOrder() | Descending order |
| nullsFirst() | Place null values first |
| nullsLast() | Place null values last |

---

# Internal Working

Collections.sort()

↓

Comparator.compare()

↓

Negative?

↓

Swap

↓

Continue

↓

Sorted List

Comparator only decides

```
Who Comes First
```

Sorting algorithm handles the actual rearrangement.

---

# Production Best Practices

✅ Use

```java
Comparator.comparing()
```

instead of manual compare methods.

---

✅ Chain comparisons using

```
thenComparing()
```

instead of nested if-else blocks.

---

✅ Use

```
nullsFirst()

nullsLast()
```

when sorting nullable fields.

---

✅ Keep Comparator logic outside model classes.

---

# Common Interview Mistakes

❌ Creating multiple Comparable implementations.

Wrong.

Only one Comparable implementation is allowed.

---

❌ Writing large Comparator classes.

Prefer

```
Comparator.comparing()

thenComparing()
```

---

❌ Ignoring null values.

Always consider

```
nullsFirst()

or

nullsLast()
```

when sorting nullable fields.

---

# Interview Questions

### Why is Comparator preferred in enterprise applications?

Because business requirements often change. Comparator allows multiple sorting strategies without modifying the model class.

---

### Can we chain multiple comparators?

Yes.

Using

```
thenComparing()
```

---

### Which is more commonly used today?

Comparator.

Especially with Java 8+, Streams, Lambdas, and Method References.

---

# Quick Revision

Need

```
Default Sorting

↓

Comparable
```

Need

```
Custom Sorting

↓

Comparator
```

Need

```
Descending

↓

reversed()
```

Need

```
Multiple Fields

↓

thenComparing()
```

Need

```
Handle Nulls

↓

nullsFirst()

nullsLast()
```

---

---

# Comparable vs Comparator (Deep Dive)

⭐⭐⭐⭐⭐ One of the most frequently asked Java interview questions.

Almost every Java Backend interview includes this comparison.

---

# Quick Interview Answer

**Comparable** is used to define the **natural ordering** of a class.

**Comparator** is used to define **custom ordering** outside the class.

---

# Complete Comparison

| Feature | Comparable | Comparator |
|----------|------------|------------|
| Package | java.lang | java.util |
| Method | compareTo(T o) | compare(T o1, T o2) |
| Location | Inside the class | Outside the class |
| Sorting Type | Natural | Custom |
| Number of Strategies | One | Unlimited |
| Lambda Support | No | Yes |
| Java 8 Friendly | Limited | Excellent |
| Production Usage | Less | More |

---

# Internal Working

Suppose

```java
employees.sort(
        Comparator.comparing(Employee::getSalary)
);
```

Execution

```
employees.sort()

↓

Comparator.compare()

↓

Negative?

↓

Swap

↓

Positive?

↓

Swap

↓

Repeat

↓

Sorted List
```

Notice

Comparator **does not sort**.

It only tells Java

```
Which Object Should Come First
```

The sorting algorithm performs the actual rearrangement.

---

# Sorting Algorithm

Interview Question ⭐

What sorting algorithm does Java use?

Answer

```
TimSort
```

---

# What is TimSort?

TimSort is a hybrid sorting algorithm derived from

```
Merge Sort

+

Insertion Sort
```

Advantages

- Stable
- Very fast
- Optimized for partially sorted data
- Used by

```
Collections.sort()

List.sort()
```

---

# Time Complexity

| Case | Complexity |
|-------|------------|
| Best | O(n) |
| Average | O(n log n) |
| Worst | O(n log n) |

---

# Stable Sorting

Interview Question ⭐

What is Stable Sorting?

Suppose

```
Rahul

Salary = 50000

--------------

Amit

Salary = 50000
```

Sorting by salary

Output

```
Rahul

Amit
```

Original order remains unchanged.

This is

```
Stable Sorting
```

TimSort is stable.

---

# Natural Ordering Example

Employee

```
Employee ID
```

Only one natural order.

```java
class Employee
implements Comparable<Employee>{

    @Override
    public int compareTo(Employee e){

        return Integer.compare(
                this.id,
                e.id
        );

    }

}
```

Now every Employee has one default ordering.

---

# Custom Ordering Example

Need

```
Salary
```

Tomorrow

```
Department
```

Tomorrow

```
Joining Date
```

Tomorrow

```
Experience
```

Solution

```
Comparator
```

---

# Production Scenario 1

Employee Portal

Requirement

```
Sort Employees

↓

Department

↓

Salary

↓

Name
```

Solution

```java
Comparator

.comparing(Employee::getDepartment)

.thenComparing(Employee::getSalary)

.thenComparing(Employee::getName);
```

---

# Production Scenario 2

E-Commerce

Requirement

```
Products

↓

Category

↓

Price

↓

Rating
```

Comparator

```java
Comparator

.comparing(Product::getCategory)

.thenComparing(Product::getPrice)

.thenComparing(Product::getRating);
```

---

# Production Scenario 3

Student Portal

Requirement

```
Marks

↓

Descending

↓

Name
```

```java
Comparator

.comparing(Student::getMarks)

.reversed()

.thenComparing(Student::getName);
```

---

# Production Scenario 4

Order Management

Requirement

```
Order Date

↓

Priority

↓

Customer Name
```

Comparator

```
Date

↓

Priority

↓

Name
```

---

# Which One Should You Use?

Need

```
Default Ordering

↓

Comparable
```

Need

```
Business Requirement

↓

Comparator
```

---

# Why Comparator is Preferred Today

Modern Java

↓

Streams

↓

Lambda

↓

Method Reference

↓

Comparator

Examples

```java
employees.stream()

.sorted(

Comparator.comparing(
Employee::getSalary)

)

.toList();
```

Much cleaner.

---

# Performance

Comparable

```
Slightly Less Flexible
```

Comparator

```
Highly Flexible
```

Performance difference is negligible.

Choose based on

```
Design

NOT

Speed
```

---

# Best Practices

✅ Use Comparable only for one obvious natural ordering.

Example

```
Employee ID

ISBN

Roll Number
```

---

✅ Use Comparator for business-specific sorting.

---

✅ Prefer

```java
Comparator.comparing()
```

instead of manual comparison logic.

---

✅ Chain comparisons using

```
thenComparing()
```

---

✅ Avoid modifying model classes just to satisfy one sorting requirement.

---

# Common Interview Mistakes

❌ Implement Comparable for every sorting requirement.

Wrong.

Comparable supports only one natural ordering.

---

❌ Put business logic inside compareTo().

Wrong.

Business rules change frequently.

Keep them in Comparators.

---

❌ Create many Comparator classes.

Modern Java allows

```java
Comparator.comparing()
```

with lambdas.

Prefer that.

---

# Architecture Decision

Model

```
Employee
```

↓

Comparable

```
Employee ID
```

Service Layer

↓

Comparator

```
Salary

Department

Experience

Joining Date
```

This keeps the model simple and business logic flexible.

---

# Interview Questions

## Why can't Comparable support multiple sorting strategies?

Because a class can implement Comparable only once and therefore has only one compareTo() method.

---

## Why is Comparator more flexible?

Because multiple Comparator instances can be created for different sorting requirements without changing the model class.

---

## Which one is used more in Spring Boot projects?

Comparator.

Business requirements often require different sorting orders in different APIs and services.

---

## Which package contains Comparable?

```
java.lang
```

---

## Which package contains Comparator?

```
java.util
```

---

## Can we use Comparator with Streams?

Yes.

Example

```java
employees.stream()

.sorted(

Comparator.comparing(
Employee::getName)

)

.toList();
```

---

# Interview Cheat Sheet

| Requirement | Choose |
|-------------|--------|
| Default ordering | Comparable |
| Multiple sorting | Comparator |
| Stream sorting | Comparator |
| Lambda sorting | Comparator |
| Business sorting | Comparator |
| Employee ID sorting | Comparable |

---

# Summary

Comparable

↓

One Natural Ordering

↓

Inside Class

↓

compareTo()

----------------------------

Comparator

↓

Multiple Custom Orderings

↓

Outside Class

↓

compare()

↓

Preferred in Modern Java

---

# Scenario-Based Interview Questions

## Scenario 1

### Requirement

```
Employee List

↓

Sort by Employee ID
```

### Best Choice

```
Comparable
```

### Why?

Employee ID is the natural identity of an employee.

---

## Scenario 2

### Requirement

```
Sort Employees

↓

Salary
```

### Best Choice

```
Comparator
```

---

## Scenario 3

### Requirement

```
Sort Employees

↓

Department

↓

Salary

↓

Name
```

### Best Choice

```java
Comparator
    .comparing(Employee::getDepartment)
    .thenComparing(Employee::getSalary)
    .thenComparing(Employee::getName);
```

---

## Scenario 4

### Requirement

```
Products

↓

Price Descending
```

Solution

```java
Comparator
    .comparing(Product::getPrice)
    .reversed();
```

---

## Scenario 5

### Requirement

```
Students

↓

Marks Descending

↓

Name Ascending
```

Solution

```java
Comparator
    .comparing(Student::getMarks)
    .reversed()
    .thenComparing(Student::getName);
```

---

## Scenario 6

### Requirement

```
Sort Nullable Employee Names
```

Solution

```java
Comparator.comparing(
    Employee::getName,
    Comparator.nullsLast(
        Comparator.naturalOrder()
    )
);
```

---

# Frequently Asked Interview Questions

## 🟢 Beginner Level

### What is Comparable?

Comparable defines the natural ordering of objects.

---

### What is Comparator?

Comparator defines custom sorting outside the class.

---

### Which method does Comparable contain?

```java
compareTo()
```

---

### Which method does Comparator contain?

```java
compare()
```

---

### Which package contains Comparable?

```text
java.lang
```

---

### Which package contains Comparator?

```text
java.util
```

---

### Can Comparable provide multiple sorting strategies?

No.

---

### Can Comparator provide multiple sorting strategies?

Yes.

---

## 🟡 Intermediate Level

### Difference between Comparable and Comparator?

| Comparable | Comparator |
|------------|------------|
| Natural Ordering | Custom Ordering |
| Inside Class | Outside Class |
| One Strategy | Multiple Strategies |
| compareTo() | compare() |

---

### Why is Comparator preferred today?

Because business requirements frequently change and Java 8 provides lambda expressions and method references that make Comparator concise and reusable.

---

### What does compareTo() return?

| Value | Meaning |
|-------|---------|
| Negative | Current object is smaller |
| Zero | Objects are equal |
| Positive | Current object is greater |

---

### What does compare() return?

Exactly the same semantics:

- Negative
- Zero
- Positive

---

### What is thenComparing()?

Used for multi-level sorting.

Example

```
Salary

↓

Name

↓

Department
```

---

### What does reversed() do?

Sorts in descending order.

---

### What is Comparator.comparing()?

Creates a Comparator using an object property.

---

### Why use nullsFirst()?

To safely sort nullable values by placing null values before non-null values.

---

### Why use nullsLast()?

To safely sort nullable values by placing null values after non-null values.

---

## 🔴 Advanced Level

### Which sorting algorithm does Java use?

For object sorting in modern JDKs:

```
TimSort
```

---

### Is TimSort stable?

Yes.

Equal elements retain their original relative order.

---

### Why is stable sorting important?

Suppose

```
Rahul

50000

--------------

Amit

50000
```

Sorting by salary keeps Rahul before Amit because their salaries are equal.

---

### Which is more commonly used in Spring Boot projects?

Comparator.

---

### Can Streams use Comparator?

Yes.

```java
employees.stream()
         .sorted(
             Comparator.comparing(Employee::getSalary)
         )
         .toList();
```

---

### Can we sort in descending order?

Yes.

```java
Comparator.comparing(
    Employee::getSalary
).reversed();
```

---

### Can we chain multiple Comparators?

Yes.

Using

```
thenComparing()
```

---

# Common Interview Mistakes

❌ Using Comparable for every sorting requirement.

Wrong.

Comparable supports only one natural ordering.

---

❌ Writing large Comparator classes.

Modern Java provides

- Lambdas
- Method References
- Comparator.comparing()

---

❌ Forgetting null handling.

Always think about

```
nullsFirst()

or

nullsLast()
```

---

❌ Modifying the model class whenever sorting changes.

Business-specific sorting belongs in Comparator.

---

# Production Decision Guide

| Requirement | Recommended Approach |
|-------------|----------------------|
| Default sorting | Comparable |
| Business sorting | Comparator |
| Multi-level sorting | Comparator |
| Stream sorting | Comparator |
| Lambda sorting | Comparator |
| Descending order | Comparator.reversed() |
| Nullable fields | nullsFirst()/nullsLast() |

---

# One-Day Revision Sheet

## Remember

✅ Comparable

- `java.lang`
- `compareTo()`
- Inside class
- One natural ordering

---

✅ Comparator

- `java.util`
- `compare()`
- Outside class
- Multiple sorting strategies

---

### Useful Methods

| Method | Purpose |
|--------|---------|
| comparing() | Sort using property |
| thenComparing() | Secondary sorting |
| reversed() | Descending order |
| naturalOrder() | Ascending order |
| reverseOrder() | Descending order |
| nullsFirst() | Null values first |
| nullsLast() | Null values last |

---

# Decision Tree

```
Need Default Ordering?

↓

YES

↓

Comparable

↓

NO

↓

Need Multiple Sorting?

↓

YES

↓

Comparator

↓

Need Descending?

↓

reversed()

↓

Need Multi-Level?

↓

thenComparing()

↓

Need Null Handling?

↓

nullsFirst()

or

nullsLast()
```

---

# 60-Second Interview Answer

> Comparable is used to define the natural ordering of a class by implementing the `compareTo()` method inside the class itself. Since a class can have only one natural ordering, Comparable supports only one sorting strategy. Comparator, on the other hand, is defined outside the class using the `compare()` method and allows multiple custom sorting strategies. In modern Java applications, Comparator is preferred because it works well with lambda expressions, method references, Streams, and methods like `comparing()`, `thenComparing()`, and `reversed()`.

---

# Chapter Summary

After completing this chapter, you should be able to:

- Explain Comparable and Comparator.
- Distinguish natural ordering from custom ordering.
- Use `Comparator.comparing()`, `thenComparing()`, and `reversed()`.
- Handle null values using `nullsFirst()` and `nullsLast()`.
- Sort collections using Streams.
- Recommend the appropriate approach for different production scenarios.
- Answer common Java backend interview questions confidently.

---

# Next Chapter

**07-streams.md**

Topics:

- What is a Stream?
- Stream Pipeline
- Stream vs Collection
- Intermediate Operations
- Terminal Operations
- filter()
- map()
- flatMap()
- reduce()
- collect()
- groupingBy()
- partitioningBy()
- collectingAndThen()
- findFirst() vs findAny()
- anyMatch() vs allMatch() vs noneMatch()
- Parallel Streams
- Performance
- Production Use Cases
- 50+ Interview Questions