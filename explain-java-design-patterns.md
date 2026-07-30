# Core Java design patterns
Absolutely — here’s the same interview-prep style for core Java design patterns, grouped by the classic GoF categories: Creational, Structural, and Behavioral.

## Creational Patterns

Creational patterns are about object creation: how objects are created, hidden, reused, or configured.

## 1. Singleton Pattern

Description: Ensures a class has only one instance and provides a global access point to it.

Use case: Use for shared resources like configuration managers, logging services, cache managers, or connection pools.

```java
public class AppConfig {

    private static final AppConfig INSTANCE = new AppConfig();

    private AppConfig() {
        // Use case:
        // Private constructor prevents other classes from creating new instances.
    }

    public static AppConfig getInstance() {
        // Use case:
        // Returns the same shared instance every time.
        return INSTANCE;
    }
}
```

## 2. Factory Method Pattern

Description: Defines a method for creating objects, but lets subclasses or logic decide which class to instantiate.

Use case: Use when object creation depends on input, type, or runtime condition.

```java
interface Notification {
    void send(String message);
}

class EmailNotification implements Notification {
    public void send(String message) {
        System.out.println("Email: " + message);
    }
}

class SmsNotification implements Notification {
    public void send(String message) {
        System.out.println("SMS: " + message);
    }
}

class NotificationFactory {

    public static Notification createNotification(String type) {
        // Use case:
        // Centralizes object creation.
        // Caller does not need to know concrete class names.
        if ("email".equalsIgnoreCase(type)) {
            return new EmailNotification();
        } else if ("sms".equalsIgnoreCase(type)) {
            return new SmsNotification();
        }

        throw new IllegalArgumentException("Unknown notification type");
    }
}
```

## 3. Abstract Factory Pattern

Description: Creates families of related objects without specifying their concrete classes.

Use case: Use when your application supports multiple product families, such as Windows UI vs Mac UI components.

```java
interface Button {
    void render();
}

interface Checkbox {
    void render();
}

class WindowsButton implements Button {
    public void render() {
        System.out.println("Windows button");
    }
}

class WindowsCheckbox implements Checkbox {
    public void render() {
        System.out.println("Windows checkbox");
    }
}

class MacButton implements Button {
    public void render() {
        System.out.println("Mac button");
    }
}

class MacCheckbox implements Checkbox {
    public void render() {
        System.out.println("Mac checkbox");
    }
}

interface UIFactory {
    Button createButton();
    Checkbox createCheckbox();
}

class WindowsUIFactory implements UIFactory {
    public Button createButton() {
        return new WindowsButton();
    }

    public Checkbox createCheckbox() {
        return new WindowsCheckbox();
    }
}

class MacUIFactory implements UIFactory {
    public Button createButton() {
        return new MacButton();
    }

    public Checkbox createCheckbox() {
        return new MacCheckbox();
    }
}
```

## 4. Builder Pattern

Description: Builds complex objects step by step instead of using long constructors.

Use case: Use when an object has many optional fields or needs readable construction.

```java
public class User {

    private String name;
    private String email;
    private int age;

    private User(Builder builder) {
        this.name = builder.name;
        this.email = builder.email;
        this.age = builder.age;
    }

    public static class Builder {
        private String name;
        private String email;
        private int age;

        public Builder name(String name) {
            this.name = name;
            return this;
        }

        public Builder email(String email) {
            this.email = email;
            return this;
        }

        public Builder age(int age) {
            this.age = age;
            return this;
        }

        public User build() {
            // Use case:
            // Creates a User after optional values have been configured.
            return new User(this);
        }
    }
}
```

Usage:

```java
User user = new User.Builder()
        .name("Julio")
        .email("julio@example.com")
        .age(30)
        .build();
```

## 5. Prototype Pattern

Description: Creates new objects by copying an existing object.

Use case: Use when object creation is expensive and copying is easier than rebuilding from scratch.

```java
class Employee implements Cloneable {

    private String name;
    private String department;

    public Employee(String name, String department) {
        this.name = name;
        this.department = department;
    }

    @Override
    public Employee clone() {
        try {
            // Use case:
            // Creates a copy of the existing Employee object.
            return (Employee) super.clone();
        } catch (CloneNotSupportedException e) {
            throw new RuntimeException(e);
        }
    }
}
```

## Structural Patterns

Structural patterns are about how classes and objects are composed to form larger structures.

## 6. Adapter Pattern

Description: Allows incompatible interfaces to work together by wrapping one interface with another.

Use case: Use when integrating legacy code or third-party APIs with your expected interface.

```java
interface PaymentProcessor {
    void pay(double amount);
}

class LegacyPaymentService {
    public void makePayment(int amountInCents) {
        System.out.println("Paid cents: " + amountInCents);
    }
}

class PaymentAdapter implements PaymentProcessor {

    private final LegacyPaymentService legacyPaymentService;

    public PaymentAdapter(LegacyPaymentService legacyPaymentService) {
        this.legacyPaymentService = legacyPaymentService;
    }

    public void pay(double amount) {
        // Use case:
        // Converts dollars into cents and delegates to legacy service.
        legacyPaymentService.makePayment((int) (amount * 100));
    }
}
```

## 7. Bridge Pattern

Description: Separates abstraction from implementation so both can vary independently.

Use case: Use when you want to avoid many subclasses caused by multiple dimensions of variation.

```java
interface Device {
    void turnOn();
    void turnOff();
}

class Tv implements Device {
    public void turnOn() {
        System.out.println("TV on");
    }

    public void turnOff() {
        System.out.println("TV off");
    }
}

class Radio implements Device {
    public void turnOn() {
        System.out.println("Radio on");
    }

    public void turnOff() {
        System.out.println("Radio off");
    }
}

class RemoteControl {

    protected Device device;

    public RemoteControl(Device device) {
        this.device = device;
    }

    public void powerOn() {
        // Use case:
        // Remote abstraction delegates to a device implementation.
        device.turnOn();
    }
}
```

## 8. Composite Pattern

Description: Treats individual objects and groups of objects uniformly.

Use case: Use for tree structures like file systems, menus, departments, or organization charts.

```java
interface FileSystemItem {
    void show();
}

class File implements FileSystemItem {

    private final String name;

    public File(String name) {
        this.name = name;
    }

    public void show() {
        System.out.println("File: " + name);
    }
}

class Folder implements FileSystemItem {

    private final List<FileSystemItem> items = new ArrayList<>();

    public void add(FileSystemItem item) {
        items.add(item);
    }

    public void show() {
        // Use case:
        // Folder treats files and subfolders the same way.
        for (FileSystemItem item : items) {
            item.show();
        }
    }
}
```

## 9. Decorator Pattern

Description: Adds behavior to an object dynamically without changing its class.

Use case: Use when you want flexible combinations of features, like adding toppings, logging, compression, or encryption.

```java
interface Coffee {
    double cost();
}

class SimpleCoffee implements Coffee {
    public double cost() {
        return 3.0;
    }
}

class MilkDecorator implements Coffee {

    private final Coffee coffee;

    public MilkDecorator(Coffee coffee) {
        this.coffee = coffee;
    }

    public double cost() {
        // Use case:
        // Adds milk cost while preserving original coffee behavior.
        return coffee.cost() + 1.0;
    }
}
```

## 10. Facade Pattern

Description: Provides a simple interface to a complex subsystem.

Use case: Use when client code should not deal with many complicated classes directly.

```java
class InventoryService {
    boolean isAvailable(String productId) {
        return true;
    }
}

class PaymentService {
    void charge(String productId) {
        System.out.println("Payment charged");
    }
}

class ShippingService {
    void ship(String productId) {
        System.out.println("Product shipped");
    }
}

class OrderFacade {

    private final InventoryService inventory = new InventoryService();
    private final PaymentService payment = new PaymentService();
    private final ShippingService shipping = new ShippingService();

    public void placeOrder(String productId) {
        // Use case:
        // Client calls one simple method instead of coordinating many services.
        if (inventory.isAvailable(productId)) {
            payment.charge(productId);
            shipping.ship(productId);
        }
    }
}
```

## 11. Flyweight Pattern

Description: Reuses shared objects to reduce memory usage.

Use case: Use when many similar objects share common state, such as characters, icons, or game objects.

```java
class FontStyle {

    private final String font;
    private final int size;

    public FontStyle(String font, int size) {
        this.font = font;
        this.size = size;
    }
}

class FontFactory {

    private static final Map<String, FontStyle> cache = new HashMap<>();

    public static FontStyle getFont(String font, int size) {
        String key = font + "-" + size;

        // Use case:
        // Reuses the same FontStyle object instead of creating duplicates.
        return cache.computeIfAbsent(key, k -> new FontStyle(font, size));
    }
}
```

## 12. Proxy Pattern

Description: Provides a placeholder or wrapper that controls access to another object.

Use case: Use for lazy loading, access control, logging, caching, or remote service calls.

```java
interface Image {
    void display();
}

class RealImage implements Image {

    private final String filename;

    public RealImage(String filename) {
        this.filename = filename;
        loadFromDisk();
    }

    private void loadFromDisk() {
        System.out.println("Loading " + filename);
    }

    public void display() {
        System.out.println("Displaying " + filename);
    }
}

class ProxyImage implements Image {

    private RealImage realImage;
    private final String filename;

    public ProxyImage(String filename) {
        this.filename = filename;
    }

    public void display() {
        // Use case:
        // Delays expensive image loading until display is actually called.
        if (realImage == null) {
            realImage = new RealImage(filename);
        }

        realImage.display();
    }
}
```

## Behavioral Patterns

Behavioral patterns are about communication, responsibility, and algorithms between objects.

## 13. Chain of Responsibility Pattern

Description: Passes a request along a chain of handlers until one handles it.

Use case: Use for logging levels, approval workflows, servlet filters, or validation chains.

```java
abstract class Handler {

    protected Handler next;

    public Handler setNext(Handler next) {
        this.next = next;
        return next;
    }

    public void handle(String request) {
        if (next != null) {
            // Use case:
            // If this handler does not process the request,
            // pass it to the next handler.
            next.handle(request);
        }
    }
}
```

## 14. Command Pattern

Description: Encapsulates a request as an object.

Use case: Use for undo/redo, task queues, job scheduling, or button actions.

```java
interface Command {
    void execute();
}

class Light {
    void turnOn() {
        System.out.println("Light on");
    }
}

class TurnOnLightCommand implements Command {

    private final Light light;

    public TurnOnLightCommand(Light light) {
        this.light = light;
    }

    public void execute() {
        // Use case:
        // Encapsulates the action of turning on a light.
        light.turnOn();
    }
}
```

## 15. Interpreter Pattern

Description: Defines a grammar and interprets sentences in that grammar.

Use case: Use for simple rule engines, expression parsers, or query languages.

```java
interface Expression {
    boolean interpret(String context);
}

class ContainsExpression implements Expression {

    private final String word;

    public ContainsExpression(String word) {
        this.word = word;
    }

    public boolean interpret(String context) {
        // Use case:
        // Checks whether context matches a simple rule.
        return context.contains(word);
    }
}
```

## 16. Iterator Pattern

Description: Provides a way to access elements of a collection without exposing its internal structure.

Use case: Use when traversing lists, trees, custom collections, or streams.

```java
List<String> names = List.of("Ana", "Ben", "Cory");

Iterator<String> iterator = names.iterator();

while (iterator.hasNext()) {
    // Use case:
    // Iterator lets us traverse the collection
    // without knowing how the list stores elements internally.
    System.out.println(iterator.next());
}
```

## 17. Mediator Pattern

Description: Centralizes communication between objects so they do not depend on each other directly.

Use case: Use in chat rooms, UI components, event systems, or workflow coordinators.

```java
class ChatRoom {

    public void sendMessage(User user, String message) {
        // Use case:
        // ChatRoom acts as the mediator.
        // Users communicate through it instead of directly with each other.
        System.out.println(user.getName() + ": " + message);
    }
}

class User {

    private final String name;
    private final ChatRoom chatRoom;

    public User(String name, ChatRoom chatRoom) {
        this.name = name;
        this.chatRoom = chatRoom;
    }

    public String getName() {
        return name;
    }

    public void send(String message) {
        chatRoom.sendMessage(this, message);
    }
}
```

## 18. Memento Pattern

Description: Captures and restores an object’s previous state without exposing internal details.

Use case: Use for undo features, checkpoints, form drafts, or editor history.

```java
class EditorMemento {

    private final String content;

    public EditorMemento(String content) {
        this.content = content;
    }

    public String getContent() {
        return content;
    }
}

class Editor {

    private String content;

    public void write(String content) {
        this.content = content;
    }

    public EditorMemento save() {
        // Use case:
        // Saves the current editor state.
        return new EditorMemento(content);
    }

    public void restore(EditorMemento memento) {
        // Use case:
        // Restores a previous editor state.
        this.content = memento.getContent();
    }
}
```

## 19. Observer Pattern

Description: Allows objects to subscribe and react when another object changes.

Use case: Use for event listeners, notifications, messaging, and UI updates.

```java
interface Observer {
    void update(String event);
}

class EmailSubscriber implements Observer {
    public void update(String event) {
        System.out.println("Email received: " + event);
    }
}

class EventPublisher {

    private final List<Observer> observers = new ArrayList<>();

    public void subscribe(Observer observer) {
        observers.add(observer);
    }

    public void publish(String event) {
        // Use case:
        // Publisher notifies all subscribers without knowing their concrete classes.
        for (Observer observer : observers) {
            observer.update(event);
        }
    }
}
```

## 20. State Pattern

Description: Allows an object to change behavior when its internal state changes.

Use case: Use for order status, workflow states, vending machines, or document approval flows.

```java
interface OrderState {
    void next(Order order);
}

class NewOrderState implements OrderState {
    public void next(Order order) {
        // Use case:
        // Behavior depends on the current state.
        order.setState(new PaidOrderState());
    }
}

class PaidOrderState implements OrderState {
    public void next(Order order) {
        order.setState(new ShippedOrderState());
    }
}

class ShippedOrderState implements OrderState {
    public void next(Order order) {
        System.out.println("Order already shipped");
    }
}

class Order {

    private OrderState state = new NewOrderState();

    public void setState(OrderState state) {
        this.state = state;
    }

    public void next() {
        state.next(this);
    }
}
```

## 21. Strategy Pattern

Description: Defines a family of algorithms and makes them interchangeable.

Use case: Use for payment methods, discount rules, validation logic, sorting, or pricing algorithms.

```java
interface DiscountStrategy {
    double applyDiscount(double amount);
}

class RegularCustomerDiscount implements DiscountStrategy {
    public double applyDiscount(double amount) {
        return amount * 0.95;
    }
}

class PremiumCustomerDiscount implements DiscountStrategy {
    public double applyDiscount(double amount) {
        return amount * 0.80;
    }
}

class CheckoutService {

    private final DiscountStrategy discountStrategy;

    public CheckoutService(DiscountStrategy discountStrategy) {
        this.discountStrategy = discountStrategy;
    }

    public double checkout(double amount) {
        // Use case:
        // CheckoutService can use different discount algorithms
        // without changing its own code.
        return discountStrategy.applyDiscount(amount);
    }
}
```

## 22. Template Method Pattern

Description: Defines the skeleton of an algorithm in a base class and lets subclasses customize specific steps.

Use case: Use when multiple classes follow the same workflow but differ in certain steps.

```java
abstract class DataProcessor {

    public final void process() {
        // Use case:
        // Fixed algorithm structure.
        // Subclasses customize individual steps.
        readData();
        transformData();
        saveData();
    }

    protected abstract void readData();

    protected abstract void transformData();

    protected void saveData() {
        System.out.println("Saving data");
    }
}

class CsvDataProcessor extends DataProcessor {

    protected void readData() {
        System.out.println("Reading CSV");
    }

    protected void transformData() {
        System.out.println("Transforming CSV data");
    }
}
```

## 23. Visitor Pattern

Description: Adds new operations to existing object structures without changing their classes.

Use case: Use when you need to perform many operations on a group of related objects, such as calculating shipping, exporting reports, or generating summaries.

```java
interface ShoppingCartItem {
    void accept(ShoppingCartVisitor visitor);
}

class Book implements ShoppingCartItem {
    private final double price;

    public Book(double price) {
        this.price = price;
    }

    public double getPrice() {
        return price;
    }

    public void accept(ShoppingCartVisitor visitor) {
        visitor.visit(this);
    }
}

interface ShoppingCartVisitor {
    void visit(Book book);
}

class PriceCalculator implements ShoppingCartVisitor {

    public void visit(Book book) {
        // Use case:
        // Adds price calculation behavior without putting it directly inside Book.
        System.out.println("Book price: " + book.getPrice());
    }
}
```

## Interview shortcut

The full GoF list has 23 classic design patterns:

Creational:

- Singleton
- Factory Method
- Abstract Factory
- Builder
- Prototype

Structural:

- Adapter
- Bridge
- Composite
- Decorator
- Facade
- Flyweight
- Proxy

Behavioral:

- Chain of Responsibility
- Command
- Interpreter
- Iterator
- Mediator
- Memento
- Observer
- State
- Strategy
- Template Method
- Visitor

If you’re short on prep time, focus first on:

- Singleton
- Factory
- Builder
- Adapter
- Decorator
- Facade
- Proxy
- Strategy
- Observer
- Template Method
- Chain of Responsibility

Those come up constantly in Java/Spring interviews.