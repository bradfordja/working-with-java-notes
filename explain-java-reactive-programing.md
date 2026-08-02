```md
# Java Reactive Programming Interview Prep

## What Is Reactive Programming?

Reactive programming is a programming style focused on handling **asynchronous data streams**.

Instead of writing code that waits for a result, reactive code reacts when data becomes available.

Common use cases:

- API calls
- Real-time notifications
- Streaming data
- User events
- Messaging systems
- High-concurrency web applications

In Java, reactive programming is commonly used with:

- Project Reactor
- RxJava
- Spring WebFlux
- Reactive Streams API

---

## Simple Interview Definition

Reactive programming is a model for building asynchronous, non-blocking applications using streams of data and events.

A good interview answer:

> Reactive programming lets a program react to data as it arrives instead of blocking while waiting. In Java, it is often implemented with libraries like Project Reactor or RxJava, using types such as `Mono`, `Flux`, `Observable`, and `Flowable`.

---

## Why Reactive Programming?

Traditional blocking code waits for each operation to finish.

Example:

```java
User user = userService.getUser(id);
Order order = orderService.getOrder(user.getId());
return order;
```

If `getUser` or `getOrder` calls a database or external API, the thread waits.

Reactive code does not block the thread while waiting.

```java
return userService.getUser(id)
    .flatMap(user -> orderService.getOrder(user.getId()));
```

This allows the application to handle more concurrent work with fewer threads.

---

## Core Concepts

## 1. Asynchronous

Reactive programming is often asynchronous.

That means the program can start a task and continue doing other work while waiting for the result.

```java
Mono<User> userMono = userService.findUserById(id);
```

The result is not available immediately. It will be emitted later.

---

## 2. Non-Blocking

Non-blocking means a thread is not stuck waiting for I/O.

Instead of this:

```java
User user = repository.findById(id); // blocks
```

Reactive code returns a publisher:

```java
Mono<User> user = repository.findById(id); // does not block
```

Important interview note:

Reactive programming is especially useful for I/O-heavy systems, not necessarily CPU-heavy systems.

---

## 3. Data Streams

A stream is a sequence of values over time.

Examples:

- One HTTP response
- Many chat messages
- Many database rows
- Many stock price updates
- Many user clicks

Reactive streams can emit:

- Zero values
- One value
- Many values
- An error
- A completion signal

---

## 4. Backpressure

Backpressure is a way for a consumer to tell a producer:

> “I cannot handle data that fast. Slow down.”

This prevents a fast producer from overwhelming a slower consumer.

Example scenario:

- A service produces 10,000 events per second
- A consumer can process only 1,000 events per second
- Backpressure helps control the flow

Backpressure is a key concept in the Reactive Streams specification.

---

## Reactive Streams API

Java reactive programming is heavily influenced by the Reactive Streams standard.

The main interfaces are:

```java
Publisher<T>
Subscriber<T>
Subscription
Processor<T, R>
```

### Publisher

Produces data.

```java
interface Publisher<T> {
    void subscribe(Subscriber<? super T> subscriber);
}
```

### Subscriber

Consumes data.

```java
interface Subscriber<T> {
    void onSubscribe(Subscription subscription);
    void onNext(T item);
    void onError(Throwable throwable);
    void onComplete();
}
```

### Subscription

Controls the relationship between publisher and subscriber.

```java
interface Subscription {
    void request(long n);
    void cancel();
}
```

### Processor

Acts as both a subscriber and publisher.

It receives data, transforms it, and publishes data.

---

## Project Reactor

Project Reactor is a popular reactive library used by Spring WebFlux.

Its two main types are:

| Type | Meaning |
|---|---|
| `Mono<T>` | Emits zero or one item |
| `Flux<T>` | Emits zero to many items |

---

## Mono

A `Mono<T>` represents a stream that emits zero or one value.

Examples:

```java
Mono<String> name = Mono.just("Alice");
```

```java
Mono<User> user = userService.findById(1);
```

Common use cases:

- One database record
- One API response
- One computed value
- Empty result

Example:

```java
Mono.just("hello")
    .map(String::toUpperCase)
    .subscribe(System.out::println);
```

Output:

```text
HELLO
```

---

## Flux

A `Flux<T>` represents a stream that emits zero to many values.

Examples:

```java
Flux<Integer> numbers = Flux.just(1, 2, 3, 4, 5);
```

```java
Flux<User> users = userService.findAll();
```

Common use cases:

- Many database rows
- Event streams
- Messages
- Continuous updates

Example:

```java
Flux.just("Alice", "Bob", "Charlie")
    .filter(name -> name.length() > 3)
    .map(String::toUpperCase)
    .subscribe(System.out::println);
```

Output:

```text
ALICE
CHARLIE
```

---

## Common Reactor Operators

| Operator | Purpose |
|---|---|
| `map` | Transform each emitted item |
| `flatMap` | Transform each item into another async publisher |
| `filter` | Keep matching items |
| `zip` | Combine multiple publishers |
| `merge` | Combine streams as values arrive |
| `concat` | Combine streams in order |
| `switchIfEmpty` | Provide fallback if empty |
| `onErrorResume` | Recover from an error |
| `doOnNext` | Run side effect for each value |
| `doOnError` | Run side effect when error occurs |
| `subscribe` | Start consuming the stream |

---

## map vs flatMap

### map

Use `map` for synchronous one-to-one transformation.

```java
Mono<String> upper = Mono.just("hello")
    .map(value -> value.toUpperCase());
```

Input:

```text
hello
```

Output:

```text
HELLO
```

### flatMap

Use `flatMap` when the transformation returns another reactive type.

```java
Mono<Order> order = userService.findById(userId)
    .flatMap(user -> orderService.findLatestOrder(user.getId()));
```

Here, `findLatestOrder` returns a `Mono<Order>`, so `flatMap` is needed.

Interview shortcut:

- `map`: value to value
- `flatMap`: value to publisher

---

## Example: Reactive Service

```java
public Mono<UserProfile> getUserProfile(String userId) {
    return userRepository.findById(userId)
        .flatMap(user ->
            orderRepository.findLatestByUserId(user.getId())
                .map(order -> new UserProfile(user, order))
        )
        .switchIfEmpty(Mono.error(new RuntimeException("User not found")));
}
```

Explanation:

- `findById` returns a `Mono<User>`
- `flatMap` calls another async method
- `map` combines the user and order into a profile
- `switchIfEmpty` handles the case where no user exists

---

## Error Handling

Reactive streams have an error signal.

Instead of using only `try/catch`, reactive code uses operators.

### onErrorReturn

Returns a default value if an error happens.

```java
Mono.just("abc")
    .map(Integer::parseInt)
    .onErrorReturn(0);
```

### onErrorResume

Switches to another publisher when an error happens.

```java
userService.findById(id)
    .onErrorResume(error -> userService.findDefaultUser());
```

### doOnError

Runs a side effect, such as logging.

```java
userService.findById(id)
    .doOnError(error -> log.error("Failed to find user", error));
```

---

## subscribe

`subscribe` starts the stream.

```java
Mono.just("Hello")
    .subscribe(System.out::println);
```

Without subscription, many reactive streams do not execute.

In Spring WebFlux controllers, you usually do not call `subscribe` manually. You return the `Mono` or `Flux`, and the framework subscribes.

Example:

```java
@GetMapping("/users/{id}")
public Mono<User> getUser(@PathVariable String id) {
    return userService.findById(id);
}
```

---

## Cold vs Hot Streams

### Cold Stream

A cold stream starts producing data for each subscriber.

```java
Flux<Integer> numbers = Flux.just(1, 2, 3);
```

Each subscriber gets the full sequence.

### Hot Stream

A hot stream produces data whether or not someone is subscribed.

Examples:

- Live stock prices
- Chat messages
- Sensor readings

Late subscribers may miss earlier values.

---

## Blocking vs Non-Blocking

Blocking code:

```java
User user = userService.findById(id).block();
```

Non-blocking code:

```java
Mono<User> user = userService.findById(id);
```

Important interview note:

Avoid calling `block()` inside reactive pipelines. It defeats the purpose of reactive programming and can cause performance problems.

---

## Threading and Schedulers

Reactive programming can control where work runs using schedulers.

Common Reactor schedulers:

| Scheduler | Use |
|---|---|
| `Schedulers.parallel()` | CPU-bound work |
| `Schedulers.boundedElastic()` | Blocking I/O work |
| `Schedulers.single()` | Single reusable thread |
| `Schedulers.immediate()` | Current thread |

Example:

```java
Mono.fromCallable(() -> blockingFileRead())
    .subscribeOn(Schedulers.boundedElastic());
```

Use `boundedElastic` when you must call blocking code from a reactive pipeline.

---

## publishOn vs subscribeOn

### subscribeOn

Controls where subscription and upstream work begins.

```java
source.subscribeOn(Schedulers.boundedElastic());
```

### publishOn

Changes the execution context for downstream operators.

```java
source.publishOn(Schedulers.parallel());
```

Simple interview answer:

- `subscribeOn` affects where the pipeline starts
- `publishOn` affects where later steps run

---

## Spring WebFlux

Spring WebFlux is Spring’s reactive web framework.

Traditional Spring MVC is usually blocking.

Spring WebFlux is designed for non-blocking request handling.

Example controller:

```java
@RestController
@RequestMapping("/users")
public class UserController {
    private final UserService userService;

    @GetMapping("/{id}")
    public Mono<User> getUser(@PathVariable String id) {
        return userService.findById(id);
    }

    @GetMapping
    public Flux<User> getUsers() {
        return userService.findAll();
    }
}
```

---

## Reactive Database Access

Reactive applications need reactive database drivers to stay non-blocking.

Examples:

- R2DBC for relational databases
- Reactive MongoDB
- Reactive Redis

Important note:

Using a blocking JDBC repository inside WebFlux can remove many benefits of reactive programming unless it is isolated on a suitable scheduler.

---

## Advantages

Reactive programming can help with:

- High concurrency
- Efficient thread usage
- Non-blocking I/O
- Streaming data
- Better handling of slow external services
- Backpressure support

---

## Disadvantages

Reactive programming can be harder because:

- Debugging is more complex
- Stack traces can be less obvious
- Learning curve is higher
- Code can become difficult to read if overused
- Blocking calls can accidentally hurt performance
- Not every library has reactive support

---

## When To Use Reactive Programming

Good use cases:

- High-concurrency APIs
- Real-time event streams
- Chat systems
- Notification systems
- Streaming dashboards
- Services with many external API calls
- Non-blocking microservices

Less useful cases:

- Simple CRUD apps with low traffic
- Mostly CPU-bound workloads
- Apps using mostly blocking libraries
- Teams unfamiliar with reactive patterns

---

## Common Interview Questions

### What is reactive programming?

Reactive programming is a style of programming based on asynchronous, non-blocking data streams. Code reacts to data, errors, and completion signals as they happen.

---

### What is the difference between Mono and Flux?

`Mono<T>` emits zero or one item.

`Flux<T>` emits zero to many items.

Example:

```java
Mono<User> user = userService.findById(id);
Flux<User> users = userService.findAll();
```

---

### What is backpressure?

Backpressure is a mechanism that allows a subscriber to control how much data it receives from a publisher. It prevents fast producers from overwhelming slow consumers.

---

### What is the difference between map and flatMap?

`map` transforms a value into another value.

`flatMap` transforms a value into another reactive publisher and flattens it.

```java
Mono<String> name = Mono.just("alice")
    .map(String::toUpperCase);

Mono<Order> order = userMono
    .flatMap(user -> orderService.findOrder(user.getId()));
```

---

### Why should you avoid block()?

`block()` waits for a reactive result synchronously. It defeats non-blocking behavior and can cause thread starvation or performance issues, especially inside reactive applications.

---

### What is a cold publisher?

A cold publisher starts emitting data separately for each subscriber. Each subscriber usually receives the full sequence from the beginning.

---

### What is a hot publisher?

A hot publisher emits data regardless of subscribers. Late subscribers may miss previously emitted items.

---

### What is subscribe used for?

`subscribe` attaches a subscriber and starts consuming the reactive stream.

In Spring WebFlux controllers, you usually return `Mono` or `Flux` instead of calling `subscribe` manually.

---

### What is onErrorResume?

`onErrorResume` handles an error by switching to another publisher.

```java
userService.findById(id)
    .onErrorResume(error -> userService.findDefaultUser());
```

---

## Good Interview Summary Answer

Java reactive programming is used to build asynchronous, non-blocking applications based on streams of data. In Java, it is commonly implemented with Project Reactor, RxJava, and Spring WebFlux. Reactor provides `Mono` for zero-or-one values and `Flux` for zero-to-many values. Reactive streams support signals for data, completion, and errors, and include backpressure so consumers can control data flow. Reactive programming is useful for high-concurrency and I/O-heavy systems, but it requires care to avoid blocking calls and unnecessary complexity.

---

## Quick Practice Example

```java
import reactor.core.publisher.Flux;

public class ReactiveExample {
    public static void main(String[] args) {
        Flux.just("java", "spring", "reactor", "webflux")
            .filter(word -> word.length() > 5)
            .map(String::toUpperCase)
            .subscribe(System.out::println);
    }
}
```

Output:

```text
SPRING
REACTOR
WEBFLUX
```

---
```