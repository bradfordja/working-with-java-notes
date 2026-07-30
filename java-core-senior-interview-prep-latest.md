# Senior Java Developer Interview Prep

## 1. Professional Introduction

Question: Tell me about your background and why you are a good fit for this role.
Sample answer:

- I am a senior full-stack and backend developer with extensive experience designing enterprise applications using Java, Spring Framework, Spring Boot, microservices, REST APIs, messaging, databases, and cloud platforms.

- In recent projects, I have developed Spring Boot microservices, modernized legacy applications, integrated external systems, optimized SQL and application performance, and deployed containerized services through CI/CD pipelines. I have worked with Java 8, 11, 17, and 21 and understand how modern Java features improve readability, concurrency, performance, and maintainability.

- I also have hands-on experience with Docker, Kubernetes, AWS EKS, Jenkins, and GitHub-based development workflows. In a Disney environment, I would focus on building reliable, scalable, secure services that can handle high transaction volumes and integrate cleanly with other enterprise platforms.

## 2. Java 11, 17, and 21
### Java 11
Java 11 is a Long-Term Support release. Important features include:
* Standardized HTTP Client API
* New String methods
* var in lambda parameters
* Files.readString() and Files.writeString()
* Optional.isEmpty()
* Running a single-file Java program
* Removal of older Java EE modules from the JDK
* Flight Recorder availability
* Z Garbage Collector as an experimental collector

### HTTP Client
```java
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;

public class ApiClient {

    private final HttpClient client = HttpClient.newHttpClient();

    public String getData(String url) throws Exception {
        HttpRequest request = HttpRequest.newBuilder()
                .uri(URI.create(url))
                .GET()
                .build();

        return client.send(
                request,
                HttpResponse.BodyHandlers.ofString()
        ).body();
    }
}
```
- Interview point:
The Java 11 HTTP Client supports HTTP/1.1, HTTP/2, synchronous calls, and asynchronous calls through CompletableFuture.

Useful String methods
```java
String value = "  Disney Streaming  ";

System.out.println(value.strip());
System.out.println(value.isBlank());
System.out.println("Java\nSpring\nScala".lines().count());
System.out.println("Java ".repeat(3));
Optional.isEmpty()
Optional<String> result = findTitle();

if (result.isEmpty()) {
    System.out.println("Title was not found");
}
```
### Java 17
Java 17 is also an LTS release. Important features finalized by or available in Java 17 include:
* Records
* Sealed classes
* Pattern matching for instanceof
* Text blocks
* Switch expressions
* Helpful NullPointerException messages
* Strong encapsulation of JDK internals

### Records
Records provide a concise way to create immutable data carriers.
```java
public record Movie(
        Long id,
        String title,
        String category
) {}
```
The compiler generates:
* Constructor
* Accessor methods
* equals()
* hashCode()
* toString()

- Interview point:
I use records for immutable request and response models, events, configuration values, and internal projections. I avoid using them blindly as mutable JPA entities.

### Sealed classes
```java
public sealed interface PaymentResult
        permits PaymentApproved, PaymentRejected {
}

public record PaymentApproved(String transactionId)
        implements PaymentResult {
}

public record PaymentRejected(String reason)
        implements PaymentResult {
}
```
### Benefit:
Sealed classes control which classes may extend or implement a type. This is useful for modeling a known set of business outcomes.
```java
Pattern matching for instanceof
public String getTitle(Object value) {
    if (value instanceof Movie movie) {
        return movie.title();
    }

    return "Unknown";
}
```
This eliminates a separate cast.

### Switch expressions
```java
public double calculateDiscount(String membership) {
    return switch (membership) {
        case "PREMIUM" -> 0.20;
        case "STANDARD" -> 0.10;
        case "TRIAL" -> 0.05;
        default -> 0.00;
    };
}
```
### Text blocks
```java
String json = """
        {
          "title": "The Lion King",
          "category": "Animation"
        }
        """;
```
### Java 21

Java 21 is an LTS release. Important features include:
* Virtual threads
* Pattern matching for switch
* Record patterns
* Sequenced collections
* Generational ZGC
* Improved concurrency capabilities
* Structured concurrency as a preview feature
* Scoped values as a preview feature

### Virtual threads
Virtual threads allow applications to handle large numbers of blocking tasks without requiring one expensive operating-system thread per task.
```java
import java.util.concurrent.Executors;

public class VirtualThreadExample {

    public static void main(String[] args) throws Exception {
        try (var executor =
                     Executors.newVirtualThreadPerTaskExecutor()) {

            for (int i = 0; i < 1_000; i++) {
                executor.submit(() -> {
                    Thread.sleep(100);
                    return "Completed";
                });
            }
        }
    }
}
```
- Senior-level explanation:
Virtual threads are especially useful for high-concurrency, I/O-bound applications that use blocking JDBC or HTTP calls. They improve scalability without forcing the application into a fully reactive programming model. They do not make CPU-intensive work faster, and I still monitor database connection pools and downstream capacity.

### Pattern matching for switch
```java
public String describe(Object result) {
    return switch (result) {
        case PaymentApproved approved ->
                "Approved: " + approved.transactionId();

        case PaymentRejected rejected ->
                "Rejected: " + rejected.reason();

        case null -> "No result";

        default -> "Unknown result";
    };
}
```
### Record patterns
```java
record Customer(String name, String membership) {}

public String describeCustomer(Object object) {
    if (object instanceof Customer(String name, String membership)) {
        return name + " has a " + membership + " membership";
    }

    return "Not a customer";
}
```
✅ Sequenced collections
import java.util.ArrayList;
import java.util.List;

List<String> titles = new ArrayList<>();
titles.add("Frozen");
titles.add("Moana");
titles.add("Encanto");

String first = titles.getFirst();
String last = titles.getLast();

titles.addFirst("Mulan");
titles.addLast("Cars");
```

✅ Java Version Comparison
Version	Features to emphasize
Java 11	HTTP Client, String enhancements, Optional.isEmpty(), file APIs
Java 17	Records, sealed classes, pattern matching, switch expressions, text blocks
Java 21	Virtual threads, record patterns, switch pattern matching, sequenced collections

✅ Short interview answer:
Java 11 improved standard APIs such as HTTP, strings, files, and Optional. Java 17 introduced stronger domain-modeling features such as records, sealed types, and pattern matching. Java 21 significantly improves high-concurrency development with virtual threads and extends pattern matching through record patterns and enhanced switch expressions.

## 3. Object-Oriented and Functional Java
Be prepared to explain:
* Encapsulation
* Abstraction
* Inheritance
* Polymorphism
* Composition over inheritance
* Immutability
* Generics
* Streams and lambdas
* Exceptions
* Concurrency
* Collections
* CompletableFuture
* JVM memory management

Stream example
```java
List<Movie> premiumMovies = movies.stream()
        .filter(movie -> movie.category().equals("PREMIUM"))
        .sorted(Comparator.comparing(Movie::title))
        .toList();
```
CompletableFuture example
```java
CompletableFuture<Movie> movieFuture =
        CompletableFuture.supplyAsync(() -> movieService.getMovie(100L));

CompletableFuture<List<String>> recommendationsFuture =
        CompletableFuture.supplyAsync(
                () -> recommendationService.getRecommendations(100L)
        );

CompletableFuture<MovieDetails> result =
        movieFuture.thenCombine(
                recommendationsFuture,
                MovieDetails::new
        );
```
- Interview point:
I use parallel asynchronous operations when they are independent. I also define timeouts, failure handling, and appropriate executors rather than relying unconditionally on the common thread pool.

## 4. Design Patterns
#### Creational patterns

### Factory Pattern
Use when object creation depends on a type or business condition.
```java
public interface NotificationSender {
    void send(String message);
}

public class EmailSender implements NotificationSender {
    public void send(String message) {
        System.out.println("Email: " + message);
    }
}

public class SmsSender implements NotificationSender {
    public void send(String message) {
        System.out.println("SMS: " + message);
    }
}

public class NotificationFactory {

    public NotificationSender create(String type) {
        return switch (type.toUpperCase()) {
            case "EMAIL" -> new EmailSender();
            case "SMS" -> new SmsSender();
            default -> throw new IllegalArgumentException(
                    "Unsupported notification type"
            );
        };
    }
}
```
### Builder Pattern
Use for constructing complex objects without large constructors.
```java
MovieRequest request = MovieRequest.builder()
        .title("Moana")
        .category("Animation")
        .language("English")
        .build();
```
### Singleton Pattern
Spring beans are singleton-scoped by default. Avoid manually implementing a singleton when dependency injection can manage its lifecycle.

#### Structural patterns

### Adapter Pattern
Use to convert an external provider’s interface into the interface your application expects.
```java
public interface ContentProvider {
    ContentMetadata findById(String id);
}

@Component
public class ExternalContentAdapter implements ContentProvider {

    private final ExternalContentClient client;

    public ExternalContentAdapter(ExternalContentClient client) {
        this.client = client;
    }

    @Override
    public ContentMetadata findById(String id) {
        ExternalResponse response = client.getContent(id);

        return new ContentMetadata(
                response.externalId(),
                response.displayName()
        );
    }
}
```
### Facade Pattern
Provides a simplified interface over several services.
```java
@Service
public class StreamingFacade {

    private final CatalogService catalogService;
    private final EntitlementService entitlementService;
    private final PlaybackService playbackService;

    public PlaybackResponse startPlayback(
            String userId,
            String contentId
    ) {
        catalogService.validateContent(contentId);
        entitlementService.verifyAccess(userId, contentId);

        return playbackService.start(userId, contentId);
    }
}
```
### Decorator Pattern
Adds behavior without modifying the original class. Common examples include logging, caching, security, and Spring AOP interceptors.

## Behavioral patterns

### Strategy Pattern
Use when an algorithm or business rule varies.
```java
public interface RecommendationStrategy {
    List<Movie> recommend(String userId);
}

@Component("history")
public class HistoryRecommendationStrategy
        implements RecommendationStrategy {

    @Override
    public List<Movie> recommend(String userId) {
        return List.of();
    }
}

@Component("trending")
public class TrendingRecommendationStrategy
        implements RecommendationStrategy {

    @Override
    public List<Movie> recommend(String userId) {
        return List.of();
    }
}
```

### Observer Pattern
Used in:
* Domain events
* Kafka consumers
* Spring application events
* Event-driven microservices

### Template Method Pattern
Defines the overall processing sequence while allowing subclasses to customize individual steps.

## 5. Architectural Patterns
Be prepared to discuss the following:

Layered Architecture
```sh
Controller → Service → Repository → Database
```
Advantages:
* Easy to understand
* Clear separation of concerns
* Appropriate for smaller services

✅ Risk:
Business logic can become tightly coupled to frameworks and persistence.

### Hexagonal Architecture

Also called Ports and Adapters.
```sh
REST/Kafka adapters
        ↓
Application use cases
        ↓
Domain model
        ↓
Database/external-service adapters
```
- Interview answer:
I use hexagonal architecture when the business logic needs to remain independent of Spring, REST, Kafka, or a particular database. Interfaces act as ports, and framework-specific implementations become adapters.

- Clean Architecture
Dependencies point inward toward the domain and application use cases.

- Event-Driven Architecture
Services publish and consume events rather than depending exclusively on synchronous calls.
Content Service → ContentPublished event → Kafka
                                      ├─ Search Service
                                      ├─ Recommendation Service
                                      └─ Analytics Service

### CQRS
Separates command/write operations from query/read operations when their models have different scaling or performance needs.

### Saga Pattern
Coordinates transactions across microservices using:
* Choreography through events
* Orchestration through a central workflow component

### Strangler Fig Pattern
Gradually replaces a legacy system by routing individual capabilities to new services.

## 6. Writing Reusable Java Libraries

✅ Key principles

A reusable Java library should provide:
* A small and stable public API
* Clear abstractions
* Minimal dependencies
* Backward compatibility
* Thread safety
* Good exception design
* Configuration instead of hard-coded values
* Documentation and usage examples
* Unit and integration tests
* Semantic versioning
* Published artifacts through Maven or Gradle

Example library interface
```java
public interface RetryExecutor {

    <T> T execute(
            CheckedSupplier<T> operation,
            RetryPolicy policy
    );
}
@FunctionalInterface
public interface CheckedSupplier<T> {
    T get() throws Exception;
}
public record RetryPolicy(
        int maximumAttempts,
        long delayMilliseconds
) {
    public RetryPolicy {
        if (maximumAttempts < 1) {
            throw new IllegalArgumentException(
                    "Maximum attempts must be at least one"
            );
        }
    }
}
```

### Library design interview answer
I begin by defining the smallest useful public contract and keeping implementation details internal. I avoid exposing framework-specific classes unless the library is intentionally tied to that framework. I support configuration, use typed exceptions, document thread-safety expectations, and maintain backward compatibility through semantic versioning.

I test the public API rather than private implementation details and publish the library as a versioned Maven artifact. If it is a Spring-specific library, I may provide auto-configuration while still keeping the core module independent of Spring.

Maven publishing concept
```xml
<groupId>com.company.platform</groupId>
<artifactId>observability-library</artifactId>
<version>1.2.0</version>
```
Versioning:
* 1.2.1: backward-compatible bug fix
* 1.3.0: backward-compatible feature
* 2.0.0: breaking API change

Avoid these library problems
* Large public APIs
* Exposing internal implementation classes
* Static global state
* Hidden network calls
* Excessive transitive dependencies
* Breaking interfaces without a major version
* Logging confidential information
* Catching exceptions and silently ignoring them

## 7. Spring Framework and Spring Boot

Spring Framework versus Spring Boot

## Spring Framework:
Provides dependency injection, MVC, data access, transactions, security, AOP, and integration capabilities.

### Spring Boot:
Builds on Spring and adds:
* Auto-configuration
* Starter dependencies
* Embedded web servers
* Externalized configuration
* Actuator endpoints
* Production-ready conventions

### Dependency injection
```java
@Service
public class CatalogService {

    private final MovieRepository movieRepository;

    public CatalogService(MovieRepository movieRepository) {
        this.movieRepository = movieRepository;
    }
}
```

- Interview point:
I prefer constructor injection because dependencies are explicit, required dependencies can be final, and the class is easier to unit test.

### REST controller
```java
@RestController
@RequestMapping("/api/v1/movies")
public class MovieController {

    private final MovieService movieService;

    public MovieController(MovieService movieService) {
        this.movieService = movieService;
    }

    @GetMapping("/{id}")
    public ResponseEntity<MovieResponse> findById(
            @PathVariable Long id
    ) {
        return ResponseEntity.ok(movieService.findById(id));
    }

    @PostMapping
    public ResponseEntity<MovieResponse> create(
            @Valid @RequestBody CreateMovieRequest request
    ) {
        MovieResponse response = movieService.create(request);

        return ResponseEntity
                .status(HttpStatus.CREATED)
                .body(response);
    }
}
```

### Global exception handling
```java
@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(MovieNotFoundException.class)
    public ResponseEntity<ApiError> handleNotFound(
            MovieNotFoundException exception
    ) {
        ApiError error = new ApiError(
                "MOVIE_NOT_FOUND",
                exception.getMessage(),
                Instant.now()
        );

        return ResponseEntity
                .status(HttpStatus.NOT_FOUND)
                .body(error);
    }
}
```

### Transactions
```java
@Service
public class SubscriptionService {

    @Transactional
    public Subscription activateSubscription(
            ActivationRequest request
    ) {
        Subscription subscription = createSubscription(request);
        createBillingRecord(subscription);
        recordAuditEvent(subscription);

        return subscription;
    }
}
```
- Senior talking point:
I keep transaction boundaries in the service layer. I avoid long-running transactions and do not hold a database transaction open while calling a remote service. For cross-service consistency, I use patterns such as Saga and transactional outbox rather than distributed database transactions.

## 8. Microservices Architecture

### Service design principles
A well-designed microservice should:
* Represent a clear business capability
* Own its data
* Be independently deployable
* Minimize coupling
* Expose versioned contracts
* Be observable
* Handle failures gracefully
* Be horizontally scalable

Possible Disney-style domains might include:
* Content catalog
* User profile
* Subscription
* Entitlement
* Playback
* Recommendation
* Search
* Notification

* Synchronous communication
Common technologies:
* REST
* GraphQL
* gRPC
Use synchronous calls when an immediate response is required.

* Asynchronous communication
Common technologies:
* Kafka
* Event streams
* Message queues

Use asynchronous communication for:
* Analytics
* Notifications
* Content publication
* Audit processing
* Cross-service state propagation
* Long-running workflows

### Resilience
Important techniques:
* Timeouts
* Retries with exponential backoff
* Circuit breakers
* Bulkheads
* Rate limiting
* Load balancing
* Health checks
* Graceful degradation
* Idempotency
* Dead-letter queues

### Resilience4j example
```java
@CircuitBreaker(
        name = "entitlementService",
        fallbackMethod = "fallbackEntitlement"
)
@Retry(name = "entitlementService")
@TimeLimiter(name = "entitlementService")
public CompletableFuture<Boolean> verifyEntitlement(
        String userId,
        String contentId
) {
    return CompletableFuture.supplyAsync(
            () -> entitlementClient.verify(userId, contentId)
    );
}

private CompletableFuture<Boolean> fallbackEntitlement(
        String userId,
        String contentId,
        Throwable throwable
) {
    return CompletableFuture.completedFuture(false);
}
```

- Important senior-level point:
I retry only transient failures and only when the operation is safe or idempotent. Retrying every failure can amplify an outage and overload a struggling downstream service.

## 9. Data Consistency and Event Processing

### Database per service
Each microservice should normally own its database or schema. Other services access its data through APIs or events rather than directly querying its tables.

### Transactional outbox
The outbox pattern prevents a database update from succeeding while the corresponding event publication fails.
One local transaction:
    1. Update business data
    2. Insert outbox event

Publisher:
    3. Read unpublished events
    4. Publish to Kafka
    5. Mark event as published

### Idempotent consumer
```java
@Transactional
public void process(ContentPublishedEvent event) {
    if (processedEventRepository.existsById(event.eventId())) {
        return;
    }

    searchIndexService.update(event);
    processedEventRepository.save(
            new ProcessedEvent(event.eventId())
    );
}
```

- Interview point:
Most message systems provide at-least-once delivery in real production conditions. Therefore, consumers must be designed to safely handle duplicate messages.

## 10. Microservice Security

Important security controls include:
* OAuth 2.0
* OpenID Connect
* JWT access tokens
* Role-based or attribute-based authorization
* API Gateway
* TLS and mutual TLS
* Secrets management
* Input validation
* Dependency scanning
* Container scanning
* Least-privilege access
* Audit logging

Spring Security example
```java
@Bean
SecurityFilterChain securityFilterChain(
        HttpSecurity http
) throws Exception {
    return http
            .csrf(csrf -> csrf.disable())
            .authorizeHttpRequests(auth -> auth
                    .requestMatchers("/actuator/health").permitAll()
                    .requestMatchers(HttpMethod.GET, "/api/v1/movies/**")
                        .hasAnyAuthority("SCOPE_catalog.read")
                    .requestMatchers(HttpMethod.POST, "/api/v1/movies/**")
                        .hasAuthority("SCOPE_catalog.write")
                    .anyRequest().authenticated()
            )
            .oauth2ResourceServer(oauth ->
                    oauth.jwt(Customizer.withDefaults())
            )
            .build();
}
```
- Interview answer:
Authentication verifies the caller’s identity, while authorization determines what that caller can do. I validate JWT signature, issuer, audience, expiration, and scopes at the resource server. I do not trust claims from an unverified token.

## 11. Observability

A production microservice should provide:
* Structured logs
* Correlation or trace IDs
* Metrics
* Distributed tracing
* Health and readiness endpoints
* Dashboards
* Alerts
* Audit records

### Spring Boot Actuator
Useful endpoints include:
/actuator/health
/actuator/info
/actuator/metrics
/actuator/prometheus

#### Correlation ID
```java
@Component
public class CorrelationIdFilter extends OncePerRequestFilter {

    @Override
    protected void doFilterInternal(
            HttpServletRequest request,
            HttpServletResponse response,
            FilterChain filterChain
    ) throws ServletException, IOException {

        String correlationId = Optional
                .ofNullable(request.getHeader("X-Correlation-ID"))
                .orElse(UUID.randomUUID().toString());

        MDC.put("correlationId", correlationId);
        response.setHeader("X-Correlation-ID", correlationId);

        try {
            filterChain.doFilter(request, response);
        } finally {
            MDC.remove("correlationId");
        }
    }
}
```
- Interview point:
I propagate correlation and trace information through HTTP headers and message metadata so a business transaction can be followed across synchronous and asynchronous services.

## 12. Testing Strategy

Testing levels
* Unit tests
* Controller slice tests
* Repository tests
* Integration tests
* Contract tests
* End-to-end tests
* Performance tests
* Security tests

Unit test example
```java
@ExtendWith(MockitoExtension.class)
class MovieServiceTest {

    @Mock
    private MovieRepository movieRepository;

    @InjectMocks
    private MovieService movieService;

    @Test
    void shouldReturnMovieWhenItExists() {
        Movie movie = new Movie(1L, "Moana", "Animation");

        when(movieRepository.findById(1L))
                .thenReturn(Optional.of(movie));

        MovieResponse result = movieService.findById(1L);

        assertEquals("Moana", result.title());
    }
}
```
### Testcontainers
```java
@Testcontainers
@SpringBootTest
class MovieRepositoryIntegrationTest {

    @Container
    static PostgreSQLContainer<?> postgres =
            new PostgreSQLContainer<>("postgres:16")
                    .withDatabaseName("catalog")
                    .withUsername("test")
                    .withPassword("test");
}
```
### Interview point:
Testcontainers gives integration tests a real disposable database or Kafka broker, which is more reliable than assuming that an in-memory substitute behaves exactly like production.

## 13. Scala Fundamentals

### What is Scala?
Scala is a JVM language combining object-oriented and functional programming. It interoperates with Java and is commonly associated with distributed-data platforms such as Apache Spark.

Key Scala features
* Runs on the JVM
* Java interoperability
* Immutable collections
* Type inference
* Case classes
* Pattern matching
* Traits
* Higher-order functions
* Option for missing values
* Functional collection processing

### Variables
```java
val company = "Disney"  // Immutable
var count = 10          // Mutable
```
Prefer val whenever possible.

### Case class
```java
case class Movie(
  id: Long,
  title: String,
  category: String
)

val movie = Movie(1, "Moana", "Animation")
```
Case classes provide structural equality, copying, pattern matching, and useful generated methods.

### Pattern matching
```java
def describe(value: Any): String = value match {
  case movie: Movie => s"Movie: ${movie.title}"
  case text: String => s"Text: $text"
  case _            => "Unknown"
}
```
### Collections
```java
val movies = List(
  Movie(1, "Moana", "Animation"),
  Movie(2, "Avengers", "Action")
)

val titles = movies
  .filter(_.category == "Animation")
  .map(_.title)
```
### Option
```java
def findMovie(id: Long): Option[Movie] = {
  if (id == 1) {
    Some(Movie(1, "Moana", "Animation"))
  } else {
    None
  }
}

val title = findMovie(1)
  .map(_.title)
  .getOrElse("Not found")
```
Trait
```java
trait ContentService {
  def findById(id: Long): Option[Movie]
}
```
* Scala interview answer
My primary expertise is Java and Spring Boot, but I am familiar with Scala’s JVM interoperability and its functional programming model. I understand immutable values, case classes, traits, pattern matching, higher-order functions, collections, and Option. My Java streams, lambdas, Optional, and functional-programming experience provides a strong foundation for working productively in Scala.

## 14. Kubernetes
```sh
Kubernetes objects to understand
Object	Purpose
Pod	Smallest executable workload
Deployment	Manages replicas and rolling updates
Service	Provides stable network access
Ingress	Routes external HTTP traffic
ConfigMap	Stores non-sensitive configuration
Secret	Stores sensitive configuration references
HPA	Automatically scales pod replicas
Namespace	Provides logical resource isolation
StatefulSet	Manages stateful workloads
Job/CronJob	Runs one-time or scheduled workloads
```
Deployment example
```sh
apiVersion: apps/v1
kind: Deployment
metadata:
  name: catalog-service
spec:
  replicas: 3
  selector:
    matchLabels:
      app: catalog-service
  template:
    metadata:
      labels:
        app: catalog-service
    spec:
      containers:
        - name: catalog-service
          image: company/catalog-service:1.4.0
          ports:
            - containerPort: 8080
          readinessProbe:
            httpGet:
              path: /actuator/health/readiness
              port: 8080
          livenessProbe:
            httpGet:
              path: /actuator/health/liveness
              port: 8080
          resources:
            requests:
              cpu: "250m"
              memory: "512Mi"
            limits:
              cpu: "1"
              memory: "1Gi"
```
Liveness versus readiness
* Liveness: Should Kubernetes restart this container?
* Readiness: Should this pod currently receive traffic?
* Startup probe: Has a slow-starting application finished starting?

✅  Senior answer:
A readiness failure removes the pod from service endpoints without necessarily restarting it. A liveness failure causes Kubernetes to restart the container. Poorly designed liveness checks can create restart loops during a temporary downstream outage.

## 15. Jenkins

Typical Jenkins pipeline
```java
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build and Test') {
            steps {
                sh './mvnw clean verify'
            }
        }

        stage('Quality Scan') {
            steps {
                sh './mvnw sonar:sonar'
            }
        }

        stage('Build Image') {
            steps {
                sh 'docker build -t catalog-service:${BUILD_NUMBER} .'
            }
        }

        stage('Security Scan') {
            steps {
                sh 'trivy image catalog-service:${BUILD_NUMBER}'
            }
        }

        stage('Publish') {
            steps {
                sh 'docker push catalog-service:${BUILD_NUMBER}'
            }
        }

        stage('Deploy') {
            steps {
                sh 'helm upgrade --install catalog-service ./helm'
            }
        }
    }
}
```
Pipeline stages to explain:
1. Checkout source
2. Compile
3. Unit tests
4. Integration tests
5. Static analysis
6. Dependency and security scans
7. Package application
8. Build container
9. Publish immutable artifact
10. Deploy
11. Run smoke tests
12. Promote or roll back

## 16. GitHub

Be prepared to discuss:
* Feature branches or trunk-based development
* Pull requests
* Code reviews
* Protected branches
* Required status checks
* Merge strategies
* Git tags and releases
* GitHub Actions
* GitHub Packages
* Dependabot
* Secret scanning
* CODEOWNERS

Recommended workflow
```sh
Feature branch
      ↓
Pull request
      ↓
Automated tests and scans
      ↓
Peer approval
      ↓
Merge into main
      ↓
Versioned artifact
      ↓
Deployment pipeline
```
✅ Senior talking point:
The same immutable artifact should move through development, test, staging, and production. I avoid rebuilding the application separately for each environment because that creates inconsistent artifacts.

## 17. Spinnaker

### What is Spinnaker?

Spinnaker is a continuous delivery platform used to deploy and promote applications across environments and cloud platforms.

Key concepts
* Application: Collection of services and deployments
* Pipeline: Automated delivery workflow
* Stage: Individual pipeline operation
* Artifact: Deployable image or manifest
* Trigger: Git, webhook, container image, or scheduled event
* Manual judgment: Human approval gate
* Bake: Produce deployment artifacts
* Deploy: Release to a target environment

Deployment strategies

### Rolling deployment
Gradually replaces existing pods.

### Blue-green deployment
Runs old and new versions simultaneously, then switches traffic.

### Canary deployment
Sends a small percentage of traffic to the new version and compares its behavior with the stable version.

### Spinnaker interview answer
I see Jenkins and Spinnaker as complementary. Jenkins handles continuous integration—compiling, testing, scanning, and publishing the immutable container image. Spinnaker handles continuous delivery—promoting that image through environments, applying approvals, executing canary or blue-green deployments, and supporting rollback.

## 18. End-to-End CI/CD Explanation
```sh
Developer pushes code to GitHub
        ↓
Pull request starts Jenkins
        ↓
Build, unit tests and integration tests
        ↓
Code-quality and security scans
        ↓
Container image is created and published
        ↓
Spinnaker detects the versioned artifact
        ↓
Deploy to Kubernetes test environment
        ↓
Automated smoke and contract tests
        ↓
Approval or automated promotion
        ↓
Canary or blue-green production deployment
        ↓
Metrics and logs validate the release
        ↓
Promote or roll back
```
✅ Sample answer:

I would store the code in GitHub and enforce pull-request reviews and required status checks. Jenkins would compile the Java service, run unit and integration tests, perform code-quality and security scanning, build a versioned Docker image, and publish it to an artifact registry.

Spinnaker would then promote that immutable image through Kubernetes environments. In production, I would use a canary or blue-green strategy, monitor technical and business metrics, and automatically or manually roll back when the release violates defined thresholds.

## 19. Likely Interview Questions and Answers

### Question 1: Why upgrade from Java 11 to Java 21?
Java 21 provides language improvements, runtime enhancements, and virtual threads. Virtual threads can substantially simplify high-concurrency, I/O-heavy services. Records and pattern matching reduce boilerplate and clarify domain models. Before upgrading, I verify Spring Boot and dependency compatibility, run regression and performance tests, review removed or strongly encapsulated APIs, and deploy gradually.

### Question 2: When would you not use microservices?
I would not choose microservices solely because they are fashionable. For a small team or a system with limited business complexity, a modular monolith can provide clearer transactions, simpler testing, and lower operational cost. I move toward microservices when independent scaling, deployment, ownership, availability, or domain boundaries justify the added complexity.

### Question 3: How do you prevent cascading failures?
I use strict connection and response timeouts, carefully limited retries with exponential backoff and jitter, circuit breakers, bulkheads, rate limits, bounded queues, and graceful fallbacks. I monitor dependency latency and error rates and avoid retrying permanent failures or non-idempotent operations without safeguards.

### Question 4: How do you manage shared code across microservices?
I share stable, cross-cutting capabilities such as observability, security integration, and standardized error handling. I avoid sharing business-domain entities because that tightly couples service releases. Shared libraries are versioned and backward-compatible, and services control when they adopt a new version.

### Question 5: How do you handle backward compatibility?
I prefer additive API and event changes. I do not remove or rename fields until consumers migrate. I use contract tests, schema validation, API versioning when necessary, and tolerant readers. Database migrations use an expand-and-contract strategy so old and new application versions can run simultaneously.

### Question 6: How do you handle zero-downtime deployment?
I use multiple replicas, readiness probes, graceful shutdown, rolling or blue-green deployment, backward-compatible API and database changes, and pre-deployment validation. The old and new versions must be capable of operating simultaneously during the transition.

### Question 7: How would you troubleshoot a slow Spring Boot service?
I first determine whether the delay is in the application, database, network, or a downstream service. I examine latency percentiles, traces, CPU, memory, garbage collection, thread states, database connection-pool usage, slow SQL, and downstream timings. I reproduce the issue with production-like data before optimizing the measured bottleneck.

### Question 8: What is your approach to code quality?
I use clear separation of responsibilities, constructor injection, immutable models where appropriate, meaningful naming, automated tests, peer review, static analysis, dependency scanning, and consistent coding standards. I focus tests on observable behavior and important failure paths rather than testing implementation details.

### Question 9: How do you handle duplicate Kafka events?
I assume duplicate delivery is possible. I assign an event ID, store processed IDs or enforce a business-level uniqueness constraint, and make the consumer idempotent. I commit offsets only after successful processing and route repeated failures to a dead-letter topic with sufficient diagnostic context.

### Question 10: How do you make Java code thread-safe?
I minimize shared mutable state, prefer immutable objects, use concurrent collections and atomic classes when appropriate, and synchronize only the smallest necessary critical section. I also consider the complete workflow because thread-safe collections do not automatically make a multi-step business operation atomic.

## 20. Behavioral Questions
Describe a production issue you resolved

Use the STAR structure:
* Situation: A high-volume report or API became slow under production load.
* Task: Identify the bottleneck and restore acceptable performance.
* Action: Review traces and metrics, examine SQL execution plans, optimize queries, reduce unnecessary data retrieval, introduce caching or asynchronous processing, and add performance tests.
* Result: Reduced response time, prevented timeouts, and improved operational visibility.

Describe a disagreement over architecture

I begin by identifying the business and operational requirements rather than arguing for a particular technology. I compare options using scalability, reliability, delivery time, team experience, cost, and maintenance. I document the decision and, when risk is high, use a small proof of concept to replace assumptions with evidence.

* How do you mentor developers?

I explain the reasoning behind design decisions, establish clear examples and standards, review code collaboratively, and give developers ownership appropriate to their experience. My goal is to help the team make consistent decisions independently rather than making myself a permanent approval bottleneck.

## 21. Senior-Level Project Talking Point

In one of my recent projects, I helped modernize enterprise applications using Spring Boot microservices, modern front-end technologies, SQL databases, and Kubernetes-based deployment. I participated across requirements, architecture, development, testing, CI/CD, deployment, and production support.

On the backend, I designed REST APIs with clear controller, service, and repository boundaries. I implemented validation, centralized exception handling, security, transaction management, and structured logging. For service communication, I evaluated synchronous APIs versus asynchronous events based on consistency, latency, and coupling requirements.

We packaged services as containers and deployed them into Kubernetes. Our pipeline compiled and tested the application, performed quality checks, built an immutable image, and promoted that image through controlled environments. I also worked on SQL optimization, application monitoring, incident diagnosis, and backward-compatible releases.

My senior responsibility was not limited to writing code. I helped make architecture decisions, reviewed implementations, mentored developers, communicated risks to stakeholders, and ensured that the system could be supported after deployment.

## 22. Questions to Ask the Interviewers

Ask three or four of these:
1. Which Java and Spring Boot versions are currently used, and is a Java 21 migration planned?
2. How are the service boundaries organized around Disney’s business domains?
3. Which workloads use synchronous APIs versus event-driven communication?
4. How do teams manage API and event-schema compatibility?
5. What deployment strategies are implemented through Spinnaker?
6. How are production releases evaluated—technical metrics, business metrics, or both?
7. What are the current challenges involving performance, reliability, or developer productivity?
8. How much ownership does this role have over architecture and production operations?
9. How are shared Java libraries governed and versioned across teams?
10. What would successful performance in the first 90 days look like?

## 23. Final Review Checklist

Before the interview, be ready to explain:
* Java 11, 17, and 21 differences
* Records, sealed classes, pattern matching, and virtual threads
* Streams, generics, collections, concurrency, and JVM memory
* Factory, Strategy, Adapter, Builder, Observer, and Facade patterns
* Layered, hexagonal, event-driven, CQRS, Saga, and outbox architectures
* Reusable Java library design and semantic versioning
* Spring dependency injection, transactions, REST, JPA, Security, and Actuator
* Microservice resilience, observability, data ownership, and idempotency
* Basic Scala syntax, traits, case classes, Option, and pattern matching
* Kubernetes deployments, probes, scaling, and configuration
* Jenkins build pipelines
* GitHub pull-request and branching practices
* Spinnaker delivery, canary deployment, blue-green deployment, and rollback

### Strong closing statement:
My strongest value is the combination of hands-on Java and Spring Boot development with senior-level architecture and delivery experience. I can design reusable components, build resilient microservices, improve CI/CD workflows, support Kubernetes deployments, and communicate technical decisions clearly to developers, architects, and business stakeholders.


#Java 
#InterviewPrep 
#FullStack 