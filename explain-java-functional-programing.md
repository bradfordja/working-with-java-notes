```md
# Java Functional Programming Interview Prep

### What Is Functional Programming?

> Functional programming is a programming style where computation is treated as the evaluation of functions.

In Java, functional programming became much more common starting with **Java 8**, which introduced:

- Lambda expressions
- Functional interfaces
- Streams
- Method references
- Optional

Java is not a purely functional language, but it supports functional programming features.

---

### Key Ideas

## 1. Functions as Values

In functional programming, functions can be passed around like data.

In Java, this is done using **functional interfaces**.

```java
Function<Integer, Integer> square = x -> x * x;

System.out.println(square.apply(5)); // 25
```

---

### Functional Interface

A **functional interface** is an interface with exactly one abstract method.

Example:

```java
@FunctionalInterface
interface Calculator {
    int calculate(int a, int b);
}
```

Usage with a lambda:

```java
Calculator add = (a, b) -> a + b;

System.out.println(add.calculate(3, 4)); // 7
```

The `@FunctionalInterface` annotation is optional, but recommended because it lets the compiler enforce the rule.

---

## Common Built-In Functional Interfaces

Java provides many functional interfaces in `java.util.function`.
```markdown
| Interface | Method | Use |
|---|---|---|
| `Predicate<T>` | `boolean test(T t)` | Checks a condition |
| `Function<T, R>` | `R apply(T t)` | Converts one value to another |
| `Consumer<T>` | `void accept(T t)` | Performs an action |
| `Supplier<T>` | `T get()` | Provides a value |
| `BiFunction<T, U, R>` | `R apply(T t, U u)` | Takes two inputs, returns one output |
| `UnaryOperator<T>` | `T apply(T t)` | Input and output are same type |
| `BinaryOperator<T>` | `T apply(T t1, T t2)` | Combines two values of same type |
```
Example:

```java
Predicate<Integer> isEven = n -> n % 2 == 0;
Function<String, Integer> length = s -> s.length();
Consumer<String> printer = s -> System.out.println(s);
Supplier<Double> random = () -> Math.random();
```

---

## Lambda Expressions

A lambda expression is a short way to write an implementation of a functional interface.

Traditional anonymous class:

```java
Runnable r = new Runnable() {
    @Override
    public void run() {
        System.out.println("Running");
    }
};
```

Lambda version:

```java
Runnable r = () -> System.out.println("Running");
```

Lambda syntax:

```java
(parameters) -> expression
```

Examples:

```java
(x) -> x * x

(a, b) -> a + b

name -> System.out.println(name)
```

---

## Method References

A method reference is a shorter form of a lambda when the lambda only calls an existing method.

Lambda:

```java
names.forEach(name -> System.out.println(name));
```

Method reference:

```java
names.forEach(System.out::println);
```

Common forms:

```java
ClassName::staticMethod
object::instanceMethod
ClassName::instanceMethod
ClassName::new
```

Examples:

```java
Function<String, Integer> parser = Integer::parseInt;

Supplier<List<String>> listSupplier = ArrayList::new;
```

---

## Streams

A Stream represents a sequence of elements that can be processed functionally.

Example:

```java
List<String> names = List.of("Alice", "Bob", "Charlie");

List<String> result = names.stream()
    .filter(name -> name.length() > 3)
    .map(String::toUpperCase)
    .toList();

System.out.println(result); // [ALICE, CHARLIE]
```

---

## Stream Operations

Stream operations are usually divided into two types:

### Intermediate Operations

These return another stream and are lazy.

Examples:

```java
filter()
map()
sorted()
distinct()
limit()
skip()
```

### Terminal Operations

These produce a result or side effect.

Examples:

```java
collect()
toList()
forEach()
reduce()
count()
anyMatch()
allMatch()
findFirst()
```

---

## Example: filter, map, collect

```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5);

List<Integer> squaresOfEvenNumbers = numbers.stream()
    .filter(n -> n % 2 == 0)
    .map(n -> n * n)
    .toList();

System.out.println(squaresOfEvenNumbers); // [4, 16]
```

Explanation:

- `filter` keeps only even numbers
- `map` transforms each number into its square
- `toList` collects the result into a list

---

## reduce

`reduce` combines stream elements into a single value.

Example:

```java
List<Integer> numbers = List.of(1, 2, 3, 4);

int sum = numbers.stream()
    .reduce(0, (a, b) -> a + b);

System.out.println(sum); // 10
```

Using method reference:

```java
int sum = numbers.stream()
    .reduce(0, Integer::sum);
```

---

## Optional

`Optional<T>` is a container that may or may not contain a value.

It helps avoid `NullPointerException`.

Example:

```java
Optional<String> name = Optional.of("Alice");

name.ifPresent(System.out::println);
```

Empty optional:

```java
Optional<String> emptyName = Optional.empty();

String result = emptyName.orElse("Default Name");

System.out.println(result); // Default Name
```

Common methods:

```java
isPresent()
ifPresent()
orElse()
orElseGet()
orElseThrow()
map()
filter()
flatMap()
```

---

## Immutability

Functional programming favors immutable data.

Immutable data means data is not changed after creation.

Example:

```java
List<String> names = List.of("Alice", "Bob");
```

This list cannot be modified.

Instead of changing existing data, functional code often creates new data.

```java
List<String> upperNames = names.stream()
    .map(String::toUpperCase)
    .toList();
```

---

## Pure Functions

A pure function has two main properties:

1. Same input always gives the same output
2. No side effects

Example of a pure function:

```java
int square(int x) {
    return x * x;
}
```

Example of an impure function:

```java
int count = 0;

int increment() {
    count++;
    return count;
}
```

The second function is impure because it modifies external state.

---

## Side Effects

A side effect happens when code changes something outside itself.

Examples:

- Modifying a global variable
- Writing to a file
- Updating a database
- Printing to console
- Mutating an object passed as an argument

Functional programming tries to reduce side effects.

---

## Functional Programming Benefits

Common interview answer:

Functional programming helps write code that is:

- More readable
- Easier to test
- Easier to parallelize
- Less error-prone
- More declarative

Instead of saying how to do each step, functional code often describes what result is wanted.

---

## Imperative vs Functional Style

Imperative style:

```java
List<String> names = List.of("Alice", "Bob", "Charlie");
List<String> result = new ArrayList<>();

for (String name : names) {
    if (name.length() > 3) {
        result.add(name.toUpperCase());
    }
}
```

Functional style:

```java
List<String> result = names.stream()
    .filter(name -> name.length() > 3)
    .map(String::toUpperCase)
    .toList();
```

The functional version is shorter and focuses on the transformation.

---

## Parallel Streams

Java streams can be processed in parallel.

```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5);

int sum = numbers.parallelStream()
    .mapToInt(Integer::intValue)
    .sum();
```

Important interview note:

Parallel streams are not always faster. They are useful when:

- The data set is large
- Operations are CPU-intensive
- The operations are independent
- There is little shared mutable state

---

## Common Interview Questions

### What is a functional interface?

A functional interface is an interface with exactly one abstract method. It can be implemented using a lambda expression.

Example:

```java
@FunctionalInterface
interface Greeting {
    void sayHello(String name);
}
```

---

### What is a lambda expression?

A lambda expression is a concise way to provide an implementation of a functional interface.

Example:

```java
Greeting greeting = name -> System.out.println("Hello " + name);
```

---

### What is the difference between map and flatMap?

`map` transforms each element into another value.

```java
List<Integer> lengths = names.stream()
    .map(String::length)
    .toList();
```

`flatMap` transforms each element into a stream and then flattens the result.

```java
List<List<String>> nested = List.of(
    List.of("A", "B"),
    List.of("C", "D")
);

List<String> flat = nested.stream()
    .flatMap(List::stream)
    .toList();

System.out.println(flat); // [A, B, C, D]
```

---

### What is the difference between intermediate and terminal operations?

Intermediate operations return another stream and are lazy.

Examples:

```java
filter()
map()
sorted()
```

Terminal operations produce a final result.

Examples:

```java
collect()
forEach()
reduce()
count()
```

---

### What does it mean that streams are lazy?

Streams do not execute intermediate operations until a terminal operation is called.

Example:

```java
List<String> names = List.of("Alice", "Bob");

names.stream()
    .filter(name -> {
        System.out.println("Filtering " + name);
        return name.length() > 3;
    });
```

Nothing prints because there is no terminal operation.

With terminal operation:

```java
names.stream()
    .filter(name -> {
        System.out.println("Filtering " + name);
        return name.length() > 3;
    })
    .toList();
```

Now the stream executes.

---

### What is a pure function?

A pure function always returns the same output for the same input and has no side effects.

Example:

```java
int add(int a, int b) {
    return a + b;
}
```

---

### What is Optional used for?

`Optional` is used to represent a value that may be present or absent. It helps avoid returning `null`.

Example:

```java
Optional<User> user = findUserById(1);

user.ifPresent(System.out::println);
```

---

## Good Interview Summary Answer

Java supports functional programming through lambda expressions, functional interfaces, streams, method references, and Optional. A functional interface has one abstract method and can be implemented using a lambda. Streams allow declarative data processing using operations like filter, map, reduce, and collect. Functional programming encourages pure functions, immutability, and avoiding side effects, which can make code easier to read, test, and maintain.

---

## Quick Practice Example

```java
import java.util.List;

public class FunctionalExample {
    public static void main(String[] args) {
        List<String> names = List.of("Alice", "Bob", "Charlie", "David");

        List<String> result = names.stream()
            .filter(name -> name.length() > 3)
            .map(String::toUpperCase)
            .sorted()
            .toList();

        System.out.println(result);
    }
}
```

Output:

```text
[ALICE, CHARLIE, DAVID]
```

---
```