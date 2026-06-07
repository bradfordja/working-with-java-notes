# Senior Java Interview Prep Guide

## Topics Covered
- Java 8+ Features
- Collections Framework
- Streams API
- Concurrency
- Exception Handling
- JVM Basics
- Object-Oriented Programming (OOP)
- Design Patterns
---

# 1. Java 8+ Features

## Overview
Java 8 introduced functional programming concepts and modernized the language significantly.

### Key Features
- Lambda Expressions
- Streams API
- Optional
- Method References
- Functional Interfaces
- CompletableFuture
- Default Interface Methods
- New Date/Time API
---

## Lambda Expressions

### Before Java 8
```java
Runnable task = new Runnable() {
    @Override
    public void run() {
        System.out.println("Running");
    }
};
```

### Java 8
```java
Runnable task = () -> System.out.println("Running");
```

### Use Cases
- Collections processing
- Event handling
- Asynchronous programming
---

## Method References

```java
customers.forEach(System.out::println);
```

Equivalent to:
```java
customers.forEach(customer -> System.out.println(customer));
```
---

## Optional

Avoids NullPointerException.

```java
Optional<Customer> customer = repository.findById(id);
Customer result = customer.orElseThrow();
```
---

## CompletableFuture

```java
CompletableFuture<String> future =
        CompletableFuture.supplyAsync(
                () -> "Customer Loaded");
System.out.println(future.get());
```
### Use Case
Calling multiple microservices simultaneously.

```java
CompletableFuture<Customer> customerFuture =
        CompletableFuture.supplyAsync(
                () -> customerService.getCustomer());
CompletableFuture<Account> accountFuture =
        CompletableFuture.supplyAsync(
                () -> accountService.getAccount());
CompletableFuture.allOf(
        customerFuture,
        accountFuture).join();
```
---

# 2. Collections Framework

## Collection Hierarchy
```text
Collection
│
├── List
│   ├── ArrayList
│   └── LinkedList
│
├── Set
│   ├── HashSet
│   └── TreeSet
│
└── Queue
    └── PriorityQueue
```
---

## ArrayList

```java
List<String> customers =
        new ArrayList<>();
customers.add("John");
customers.add("Mary");
```

### Characteristics
- Ordered
- Allows duplicates
- Fast reads
- Slower middle insertions

### Time Complexity
| Operation | Complexity |
|------------|------------|
| get() | O(1) |
| add() | O(1) |
| insert middle | O(n) |
| remove middle | O(n) |
---

## LinkedList

```java
List<String> customers =
        new LinkedList<>();
```

### Characteristics
- Fast insertions
- Slow random access
- Doubly linked list

### Time Complexity
| Operation | Complexity |
|------------|------------|
| get() | O(n) |
| insert | O(1) |
| delete | O(1) |
---

## HashMap

```java
Map<Long, Customer> customers =
        new HashMap<>();
customers.put(1L, customer);
```

### Characteristics
- Key-value storage
- Allows one null key
- Not thread-safe
- O(1) average lookup
---

## ConcurrentHashMap

```java
Map<Long, Customer> customers =
        new ConcurrentHashMap<>();
```

### Characteristics
- Thread-safe
- Supports concurrent updates
- Used in multi-threaded applications
---

# 3. Streams API

## What is a Stream?

Streams process collections declaratively.
Traditional:
```java
List<String> results =
        new ArrayList<>();
for(String name : names) {
    if(name.startsWith("J")) {
        results.add(name);
    }
}
```

Using Streams:
```java
List<String> results =
        names.stream()
             .filter(name -> name.startsWith("J"))
             .toList();
```
---

## Filter

```java
customers.stream()
         .filter(Customer::isActive)
         .forEach(System.out::println);
```
---

## Map
```java
customers.stream()
         .map(Customer::getName)
         .forEach(System.out::println);
```
---

## Sorted
```java
customers.stream()
         .sorted(
             Comparator.comparing(
                 Customer::getName))
         .forEach(System.out::println);
```
---

## Collect
```java
List<String> names =
        customers.stream()
                 .map(Customer::getName)
                 .toList();
```
---

## Find Second Largest Distinct Value

```java
int secondLargest =
        Arrays.stream(nums)
              .distinct()
              .boxed()
              .sorted((a, b) -> b - a)
              .skip(1)
              .findFirst()
              .orElseThrow();
```

### Complexity
```text
Time: O(n log n)
Space: O(n)
```
---

# 4. Concurrency

## Runnable

```java
Thread thread =
        new Thread(
            () -> System.out.println("Running"));
thread.start();
```
---

## ExecutorService

```java
ExecutorService executor =
        Executors.newFixedThreadPool(5);
executor.submit(
        () -> System.out.println("Task"));
```
---

## CompletableFuture

```java
CompletableFuture<Customer> future =
        CompletableFuture.supplyAsync(
                () -> loadCustomer());
```
---

## Synchronization
```java
public synchronized void increment() {
    counter++;
}
```
---

## Lock
```java
Lock lock = new ReentrantLock();
lock.lock();
try {
    counter++;
}
finally {
    lock.unlock();
}
```
---

# 5. Exception Handling

## Checked Exceptions

```java
IOException
SQLException
```

```java
try {
    Files.readString(path);
}
catch(IOException ex) {
    ex.printStackTrace();
}
```
---

## Unchecked Exceptions

```java
NullPointerException
IllegalArgumentException
IndexOutOfBoundsException
```
---
## Custom Exception
```java
public class CustomerNotFoundException
        extends RuntimeException {
    public CustomerNotFoundException(
            String message) {
        super(message);
    }
}
```
---
## Global Exception Handler
```java
@RestControllerAdvice
public class GlobalExceptionHandler {
    @ExceptionHandler(
        CustomerNotFoundException.class)
    public ResponseEntity<String> handle(
        CustomerNotFoundException ex) {
        return ResponseEntity
                .badRequest()
                .body(ex.getMessage());
    }
}
```
---
# Top Senior Java Questions
1. What are Java Streams?
2. Difference between map() and flatMap()?
3. What is Optional?
4. What is CompletableFuture?
5. Difference between ArrayList and LinkedList?
6. Difference between HashMap and ConcurrentHashMap?
7. How does HashMap work internally?
8. What is synchronization?
9. What is a deadlock?
10. Difference between checked and unchecked exceptions?
11. What is garbage collection?
12. Difference between Heap and Stack?
13. Explain OOP principles.
14. What is polymorphism?
15. What is abstraction?
16. What is a Singleton?
17. What is a Factory Pattern?
18. What is a Strategy Pattern?
19. Explain Dependency Injection.
20. Explain Spring Boot auto-configuration.