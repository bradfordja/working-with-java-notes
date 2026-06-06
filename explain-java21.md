Below is a copy-ready .md document.

Java 21 New Features — Interview Prep

Java 21 is a Long-Term Support (LTS) release that introduces major improvements in concurrency, pattern matching, collections, and JVM performance. These features are especially important for modern Spring Boot, microservices, and cloud-native applications.

⸻

1. Virtual Threads (Final Feature)

Description

Virtual Threads are lightweight threads managed by the JVM instead of the operating system. They allow applications to handle thousands or even millions of concurrent tasks with minimal resource usage.

Traditional Thread

ExecutorService executor =
    Executors.newFixedThreadPool(100);

Java 21 Virtual Thread

try (var executor =
         Executors.newVirtualThreadPerTaskExecutor()) {
    executor.submit(() -> {
        System.out.println(
            Thread.currentThread()
        );
    });
}

Use Cases

* REST APIs
* Spring Boot Microservices
* Database calls
* Message processing
* High-concurrency applications

Interview Tip

Virtual Threads are part of Project Loom and solve the “thread-per-request” scalability problem.

⸻

2. Pattern Matching for switch (Final Feature)

Description

Allows switch statements to work with object types instead of only primitive values and enums.

Example

public String format(Object obj) {
    return switch (obj) {
        case Integer i ->
            "Integer: " + i;
        case String s ->
            "String: " + s;
        case null ->
            "Null";
        default ->
            "Unknown";
    };
}

Use Cases

* API request processing
* Event routing
* Command handlers

Interview Tip

Eliminates long chains of instanceof checks.

⸻

3. Record Patterns (Final Feature)

Description

Allows extracting values directly from Record objects.

Record Definition

record Person(
    String name,
    int age
) {}

Example

Object obj =
    new Person("Julio", 56);
if (obj instanceof Person(
        String name,
        int age
    )) {
    System.out.println(name);
    System.out.println(age);
}

Use Cases

* DTO processing
* API responses
* Domain-driven design

⸻

4. Sequenced Collections (Final Feature)

Description

Provides a standard API for ordered collections.

Example

List<String> names =
    new ArrayList<>();
names.add("Alice");
names.add("Bob");
System.out.println(
    names.getFirst()
);
System.out.println(
    names.getLast()
);

New Methods

getFirst()
getLast()
addFirst()
addLast()
removeFirst()
removeLast()
reversed()

Use Cases

* Queues
* Audit logs
* Event history
* Ordered reports

⸻

5. String Templates (Preview)

Description

Provides built-in string interpolation.

Traditional Java

String name = "Julio";
String message =
    "Hello " + name;

Java 21

String name = "Julio";
String message =
    STR."Hello \{name}";

Use Cases

* Logging
* SQL generation
* HTML generation
* Dynamic messages

⸻

6. Unnamed Variables and Patterns (Preview)

Description

Allows ignored variables to be represented using _.

Example

try {
    process();
}
catch (Exception _) {
    System.out.println(
        "Error occurred"
    );
}

Use Cases

* Exception handling
* Pattern matching
* Cleaner code

⸻

7. Unnamed Classes and Instance Main Methods (Preview)

Description

Simplifies small Java programs.

Traditional Java

public class Main {
    public static void main(
        String[] args
    ) {
        System.out.println(
            "Hello World"
        );
    }
}

Java 21

void main() {
    System.out.println(
        "Hello World"
    );
}

Use Cases

* Learning Java
* Scripts
* Coding exercises

⸻

8. Scoped Values (Preview)

Description

A safer replacement for ThreadLocal.

Example

static final ScopedValue<String>
    USER =
        ScopedValue.newInstance();
ScopedValue.where(
    USER,
    "Julio"
).run(() -> {
    System.out.println(
        USER.get()
    );
});

Use Cases

* User Context
* Security Context
* Correlation IDs
* Request Tracking

Interview Tip

Scoped Values work particularly well with Virtual Threads.

⸻

9. Structured Concurrency (Preview)

Description

Treats multiple concurrent tasks as a single unit of work.

Example

try (
    var scope =
        new StructuredTaskScope
            .ShutdownOnFailure()
) {
    var customer =
        scope.fork(
            () -> loadCustomer()
        );
    var orders =
        scope.fork(
            () -> loadOrders()
        );
    scope.join();
    scope.throwIfFailed();
    System.out.println(
        customer.get()
    );
    System.out.println(
        orders.get()
    );
}

Use Cases

* Aggregating multiple APIs
* Dashboard applications
* Microservice orchestration

Interview Tip

Structured Concurrency makes parallel programming easier and safer.

⸻

10. Generational ZGC

Description

Improves Java’s low-latency garbage collector by separating objects into generations.

Benefits

* Faster garbage collection
* Better throughput
* Reduced memory overhead
* Lower pause times

Use Cases

* Trading platforms
* Real-time analytics
* Cloud-native applications
* Large-scale APIs

⸻

11. Key Encapsulation Mechanism (KEM) API

Description

Introduces a standard API for cryptographic key exchange.

Example

// Used internally by
// security frameworks
// and encryption libraries

Use Cases

* Secure communications
* TLS implementations
* Encryption systems

Interview Tip

KEM helps Java prepare for modern and future cryptographic algorithms.

⸻

12. Foreign Function & Memory API (Third Preview)

Description

Provides a safer and faster replacement for JNI (Java Native Interface).

Example

MemorySegment segment =
    Arena.ofAuto()
         .allocate(100);

Use Cases

* Native libraries
* C/C++ integrations
* High-performance systems

Benefits

* Better performance
* Safer memory management
* Reduced JNI complexity

⸻

Top Java 21 Interview Questions

Q1. What is the most important Java 21 feature?

Answer:

Virtual Threads because they dramatically improve scalability for server-side applications.

⸻

Q2. What project introduced Virtual Threads?

Answer:

Project Loom.

⸻

Q3. What is Structured Concurrency?

Answer:

A programming model that treats multiple concurrent tasks as one logical operation.

⸻

Q4. Why use Scoped Values instead of ThreadLocal?

Answer:

Scoped Values are:

* Immutable
* Safer
* Better for Virtual Threads
* Easier to reason about

⸻

Q5. What are Record Patterns?

Answer:

A mechanism for extracting data directly from Records during pattern matching.

⸻

Q6. What is Pattern Matching for switch?

Answer:

Allows switch statements to match based on object types and patterns.

⸻

Q7. What is the Foreign Function & Memory API?

Answer:

A modern replacement for JNI that allows Java to interact with native code safely and efficiently.

⸻

Senior Java Developer Summary

The Java 21 features most likely to appear in a Senior Java or Spring Boot interview are:

1. Virtual Threads
2. Structured Concurrency
3. Scoped Values
4. Pattern Matching for switch
5. Record Patterns
6. Sequenced Collections
7. Generational ZGC
8. Foreign Function & Memory API

For backend development, focus heavily on:

* Virtual Threads
* Structured Concurrency
* Scoped Values
* Generational ZGC

These features directly impact scalability, performance, and cloud-native application design.

You can save this directly as:

java21-interview-prep.md