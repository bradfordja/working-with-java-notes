# Spring design-pattern list
Absolutely — here’s the improved Spring design-pattern list with a short description and commented use case for each one.

Small note: you wrote “Sprint,” but I’m assuming you meant “Spring.” Tiny typo gremlin. Happens to all of us.

## 1. Dependency Injection / Inversion of Control

Description: Spring creates and provides objects instead of your classes creating dependencies directly.

Use case: Use when one class depends on another service, repository, client, or component.

```java
@Service
public class OrderService {

    private final PaymentService paymentService;

    // Use case:
    // Spring injects PaymentService here, so OrderService does not create it manually.
    // This makes the code easier to test and loosely coupled.
    public OrderService(PaymentService paymentService) {
        this.paymentService = paymentService;
    }
}
```

## 2. Singleton Pattern

Description: Spring creates one shared instance of a bean by default.

Use case: Use for stateless services, repositories, controllers, and configuration components.

```java
@Service
public class UserService {

    // Use case:
    // Spring creates only one UserService bean by default.
    // Good for stateless business logic shared across the application.
}
```

## 3. Factory Pattern

Description: Spring acts as a factory that creates and manages objects for you.

Use case: Use when object creation should be centralized and managed by the framework.

```java
ApplicationContext context =
        new AnnotationConfigApplicationContext(AppConfig.class);

// Use case:
// ApplicationContext works like a factory.
// It creates, configures, and returns Spring-managed objects.
UserService userService = context.getBean(UserService.class);
```

## 4. Proxy Pattern

Description: Spring wraps objects with proxy objects to add extra behavior.

Use case: Used in transactions, security, caching, logging, and AOP.

```java
@Service
public class BankService {

    @Transactional
    public void transferMoney() {
        // Use case:
        // Spring creates a proxy around this service.
        // The proxy starts a transaction before the method
        // and commits or rolls back after the method.
    }
}
```

## 5. Template Method Pattern

Description: Spring provides reusable workflow templates where you only fill in custom logic.

Use case: Use when repeated boilerplate should be handled by Spring.

```java
List<User> users = jdbcTemplate.query(
        "SELECT * FROM users",
        (rs, rowNum) -> new User(rs.getLong("id"), rs.getString("name"))
);

// Use case:
// JdbcTemplate handles opening connections, executing SQL,
// processing errors, and closing resources.
// You only provide the query and row-mapping logic.
```

## 6. Front Controller Pattern

Description: A single controller receives all incoming web requests and routes them.

Use case: Spring MVC uses `DispatcherServlet` to route requests to the correct controller.

```java
@RestController
public class UserController {

    @GetMapping("/users")
    public List<User> getUsers() {
        // Use case:
        // DispatcherServlet receives the HTTP request first,
        // then routes /users to this method.
        return List.of();
    }
}
```

## 7. MVC Pattern

Description: Separates an application into Model, View, and Controller layers.

Use case: Use to organize web applications cleanly.

```java
@Controller
public class HomeController {

    @GetMapping("/")
    public String home(Model model) {
        // Use case:
        // Controller handles the request.
        // Model stores data for the view.
        // View renders the response.
        model.addAttribute("message", "Welcome");
        return "home";
    }
}
```

## 8. Strategy Pattern

Description: Defines multiple interchangeable implementations of the same behavior.

Use case: Use when business logic can vary, such as payment methods, notification methods, or sorting rules.

```java
public interface PaymentStrategy {
    void pay();
}

@Component("creditCard")
class CreditCardPayment implements PaymentStrategy {
    public void pay() {
        // Use case:
        // One payment behavior implementation.
    }
}

@Component("paypal")
class PaypalPayment implements PaymentStrategy {
    public void pay() {
        // Use case:
        // Another interchangeable payment behavior.
    }
}

@Service
public class OrderService {

    private final PaymentStrategy paymentStrategy;

    public OrderService(@Qualifier("paypal") PaymentStrategy paymentStrategy) {
        // Use case:
        // Spring injects the selected strategy.
        // You can switch behavior without changing OrderService logic.
        this.paymentStrategy = paymentStrategy;
    }
}
```

## 9. Observer / Event Listener Pattern

Description: Objects can publish events, and other objects can react without tight coupling.

Use case: Use for side effects like sending emails, logging audit records, or notifications.

```java
@Component
public class OrderEventListener {

    @EventListener
    public void handleOrderCreated(OrderCreatedEvent event) {
        // Use case:
        // This listener reacts after an order is created.
        // The order service does not need to directly call email or notification logic.
    }
}
```

## 10. Adapter Pattern

Description: Converts one interface into another expected interface.

Use case: Used internally by Spring MVC and when integrating third-party libraries.

```java
@Controller
public class LegacyUserController {

    @RequestMapping("/legacy-users")
    public String getUsers() {
        // Use case:
        // Spring MVC uses HandlerAdapter internally
        // to adapt different controller types into a common request-handling flow.
        return "users";
    }
}
```

## 11. Repository Pattern

Description: Encapsulates database access behind a clean interface.

Use case: Use when working with persistence logic in Spring Data JPA.

```java
@Repository
public interface UserRepository extends JpaRepository<User, Long> {

    // Use case:
    // Spring Data JPA generates the query implementation automatically.
    // Service classes do not need to know low-level database details.
    Optional<User> findByEmail(String email);
}
```

## 12. Builder Pattern

Description: Builds complex objects step by step.

Use case: Use when objects have many fields or optional values.

```java
User user = User.builder()
        .name("Julio")
        .email("julio@example.com")
        .active(true)
        .build();

// Use case:
// Builder makes object creation more readable,
// especially when a class has many fields.
```

With Lombok:

```java
@Builder
public class User {
    private String name;
    private String email;
    private boolean active;
}
```

## 13. DAO Pattern

Description: Separates low-level database operations from business logic.

Use case: Use when you want explicit control over SQL or persistence details.

```java
@Repository
public class UserDao {

    public User findById(Long id) {
        // Use case:
        // DAO contains database-specific logic.
        // Service layer calls this instead of writing SQL directly.
        return null;
    }
}
```

## 14. Service Layer Pattern

Description: Places business logic in service classes instead of controllers or repositories.

Use case: Use to keep controllers thin and centralize business rules.

```java
@Service
public class AccountService {

    public void openAccount(User user) {
        // Use case:
        // Business rules belong here,
        // not inside the controller or repository.
    }
}
```

## 15. Chain of Responsibility Pattern

Description: Passes a request through a chain of handlers, where each handler can process or pass it along.

Use case: Used in filters, interceptors, and Spring Security.

```java
@Component
public class AuthFilter extends OncePerRequestFilter {

    @Override
    protected void doFilterInternal(
            HttpServletRequest request,
            HttpServletResponse response,
            FilterChain filterChain
    ) throws ServletException, IOException {

        // Use case:
        // This filter can check authentication,
        // then pass the request to the next filter in the chain.
        filterChain.doFilter(request, response);
    }
}
```

## Best interview answer order

If asked, “What design patterns are used in Spring?”, say:

1. Dependency Injection / IoC
2. Singleton
3. Factory
4. Proxy
5. Template Method
6. Front Controller
7. MVC
8. Strategy
9. Observer
10. Repository

A strong short answer would be:

“Spring heavily uses Dependency Injection and IoC to manage object creation. Beans are singleton by default, the container acts like a factory, AOP and transactions use proxies, Spring MVC uses the Front Controller pattern through DispatcherServlet, and Spring also uses template classes like JdbcTemplate to reduce boilerplate.”