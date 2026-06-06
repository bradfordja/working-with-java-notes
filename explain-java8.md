Below is a copy-ready .md document.

Java 8 New Features — Interview Prep

Java 8 is one of the most important Java releases ever made. It introduced functional programming concepts, Stream APIs, and several language improvements that are heavily used in modern Java and Spring Boot applications.

⸻

1. Lambda Expressions

Description

Lambda expressions provide a concise way to implement a functional interface without creating anonymous inner classes.

Before Java 8

Runnable task = new Runnable() {
    @Override
    public void run() {
        System.out.println("Running...");
    }
};

Java 8

Runnable task = () -> System.out.println("Running...");

Use Cases

* Stream processing
* Event handlers
* Multi-threading
* Collection operations

Interview Tip

Lambda expressions reduce boilerplate code and enable functional programming in Java.

⸻

2. Functional Interfaces

Description

A Functional Interface contains exactly one abstract method.

Example

@FunctionalInterface
public interface Calculator {
    int add(int a, int b);
}

Using Lambda:

Calculator calc = (a, b) -> a + b;
System.out.println(calc.add(10, 20));

Common Functional Interfaces

Predicate<T>
Function<T,R>
Consumer<T>
Supplier<T>

Use Cases

* Stream API
* Custom business rules
* Event processing

⸻

3. Stream API

Description

Provides a functional way to process collections.

Example

List<String> names =
    Arrays.asList("John", "Jane", "Bob");
names.stream()
     .filter(name -> name.startsWith("J"))
     .forEach(System.out::println);

Output:

John
Jane

Common Operations

filter()
map()
sorted()
distinct()
limit()
collect()
reduce()

Use Cases

* Data transformation
* Reporting
* Aggregation
* Analytics

⸻

4. Method References

Description

A shorter form of Lambda expressions.

Lambda

names.forEach(name -> System.out.println(name));

Method Reference

names.forEach(System.out::println);

Types

Static Method

ClassName::methodName

Instance Method

object::methodName

Constructor

Person::new

Use Cases

* Stream processing
* Cleaner code

⸻

5. Default Methods in Interfaces

Description

Interfaces can now contain method implementations.

Example

public interface Vehicle {
    default void start() {
        System.out.println("Vehicle Started");
    }
}

Implementation:

public class Car implements Vehicle {
}

Usage:

Car car = new Car();
car.start();

Use Cases

* API evolution
* Backward compatibility

⸻

6. Static Methods in Interfaces

Description

Interfaces can contain static utility methods.

Example

public interface MathUtil {
    static int square(int n) {
        return n * n;
    }
}

Usage:

int result = MathUtil.square(5);

Use Cases

* Utility methods
* Shared logic

⸻

7. Optional Class

Description

Helps prevent NullPointerException.

Before Java 8

if(user != null) {
    System.out.println(user.getName());
}

Java 8

Optional<User> user =
    Optional.ofNullable(findUser());
user.ifPresent(
    u -> System.out.println(u.getName())
);

Useful Methods

isPresent()
ifPresent()
orElse()
orElseGet()
orElseThrow()
map()
filter()

Use Cases

* Repository methods
* API responses
* Null-safe programming

⸻

8. Date and Time API (java.time)

Description

Replaces old Date and Calendar APIs.

Example

LocalDate today =
    LocalDate.now();
System.out.println(today);

Date + Time

LocalDateTime now =
    LocalDateTime.now();

Add Days

LocalDate future =
    today.plusDays(30);

Use Cases

* Scheduling
* Financial systems
* Reporting

Interview Tip

The Java 8 Date/Time API is immutable and thread-safe.

⸻

9. forEach() Method

Description

Allows iteration using Lambdas.

Example

List<String> names =
    Arrays.asList("A", "B", "C");
names.forEach(
    name -> System.out.println(name)
);

Use Cases

* Collection processing
* Stream operations

⸻

10. Collectors API

Description

Used with Streams to aggregate data.

Example

List<String> names =
    Arrays.asList("John", "Jane");
List<String> result =
    names.stream()
         .map(String::toUpperCase)
         .collect(Collectors.toList());

Common Collectors

toList()
toSet()
joining()
groupingBy()
partitioningBy()
counting()

Example

Map<Integer, List<String>> grouped =
    names.stream()
         .collect(
             Collectors.groupingBy(
                 String::length
             )
         );

⸻

11. Parallel Streams

Description

Processes stream operations using multiple CPU cores.

Example

numbers.parallelStream()
       .forEach(System.out::println);

Use Cases

* Large datasets
* Data analytics
* Batch processing

Interview Tip

Parallel Streams use the ForkJoinPool internally.

⸻

12. Nashorn JavaScript Engine

Description

Allows JavaScript execution inside Java.

Example

ScriptEngine engine =
    new ScriptEngineManager()
        .getEngineByName("nashorn");
engine.eval(
    "print('Hello JavaScript');"
);

Use Cases

* Scripting
* Rules engines

Note: Nashorn was removed in later Java versions.

⸻

13. CompletableFuture

Description

Provides asynchronous programming support.

Example

CompletableFuture<String> future =
    CompletableFuture.supplyAsync(
        () -> "Hello"
    );
System.out.println(future.get());

Chaining

CompletableFuture.supplyAsync(
        () -> "Hello"
    )
    .thenApply(
        result -> result + " World"
    )
    .thenAccept(
        System.out::println
    );

Use Cases

* REST API aggregation
* Async database calls
* Microservices

⸻

14. Base64 API

Description

Built-in support for Base64 encoding and decoding.

Example

Encode

String encoded =
    Base64.getEncoder()
          .encodeToString(
              "Hello".getBytes()
          );

Decode

String decoded =
    new String(
        Base64.getDecoder()
              .decode(encoded)
    );

Use Cases

* JWT Tokens
* File transfers
* Authentication

⸻

15. Type Annotations

Description

Allows annotations on any type usage.

Example

List<@NotNull String> names;

Use Cases

* Validation
* Static analysis tools

⸻

Top Java 8 Interview Questions

Q1. What are Lambda Expressions?

A concise way to implement Functional Interfaces.

⸻

Q2. What is a Functional Interface?

An interface containing exactly one abstract method.

⸻

Q3. What is Stream API?

A functional approach to processing collections.

⸻

Q4. What is Optional?

A container object that may or may not contain a value.

⸻

Q5. Difference Between map() and flatMap()?

map()

Returns one object.

stream.map(String::toUpperCase);

flatMap()

Flattens nested collections.

stream.flatMap(List::stream);

⸻

Q6. What is CompletableFuture?

An API for asynchronous programming and task composition.

⸻

Q7. Why is the Date/Time API Better?

Because it is:

* Immutable
* Thread-safe
* Easier to use

⸻

Senior Java Developer Summary

The Java 8 features most frequently discussed during interviews are:

1. Lambda Expressions
2. Functional Interfaces
3. Stream API
4. Optional
5. CompletableFuture
6. Method References
7. Date/Time API
8. Collectors
9. Parallel Streams
10. Default Methods

For Spring Boot interviews, focus heavily on:

* Streams
* Optional
* Functional Interfaces
* CompletableFuture
* Lambda Expressions

These features appear regularly in modern Java, Spring Boot, Microservices, and REST API development.

You can save this directly as java8-interview-prep.md.