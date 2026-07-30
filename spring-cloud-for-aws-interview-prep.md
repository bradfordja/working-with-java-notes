# Spring Cloud Core Concepts with AWS - Interview Prep

## 1. Big Picture

Spring Cloud is a set of Spring projects that helps developers build distributed systems and microservices. It provides patterns such as externalized configuration, service discovery, load balancing, circuit breakers, API gateways, messaging, distributed tracing, and cloud-native deployment support.

When using AWS, some Spring Cloud patterns are implemented by AWS managed services instead of self-hosted Spring Cloud components. For example, instead of running a Eureka server for service discovery, a team might use ECS service discovery, AWS Cloud Map, Kubernetes service discovery on EKS, or an Application Load Balancer.

Spring Cloud AWS, now commonly used through the `io.awspring.cloud` project, gives Spring Boot applications idiomatic integration with AWS services. It wraps AWS SDK concepts in Spring-friendly auto-configuration, dependency injection, configuration properties, messaging abstractions, and resource loading.

Interview sound bite:

> Spring Cloud provides distributed-system patterns for Spring applications. On AWS, those patterns are often backed by managed services like Parameter Store, Secrets Manager, SQS, SNS, CloudWatch, S3, DynamoDB, ECS, EKS, and load balancers. Spring Cloud AWS reduces AWS SDK boilerplate and makes AWS services feel natural inside a Spring Boot app.

## 2. Spring Cloud vs Spring Cloud AWS

Spring Cloud is the broader ecosystem. It includes patterns and projects for:

- Configuration management
- Service discovery
- Client-side load balancing
- API gateway routing
- Resilience and circuit breakers
- Distributed tracing
- Messaging and event-driven systems
- Stream processing

Spring Cloud AWS focuses specifically on AWS service integration. It helps Spring Boot apps interact with services such as:

- AWS SQS
- AWS SNS
- AWS S3
- AWS Parameter Store
- AWS Secrets Manager
- AWS DynamoDB
- AWS CloudWatch

Important naming note:

- Older Spring Cloud AWS artifacts used the historical Spring Cloud AWS project.
- Modern Spring Cloud AWS development is under the `awspring` project with Maven group `io.awspring.cloud`.
- For Spring Boot 3.x and newer applications, interviewers usually expect familiarity with the newer `io.awspring.cloud` generation rather than only the old `org.springframework.cloud` style.

## 3. Core Concept: Externalized Configuration

Externalized configuration means keeping environment-specific values outside the application code.

Examples:

- Database URLs
- Feature flags
- API endpoints
- Timeouts
- Queue names
- Secret names
- Region settings

In a traditional Spring Cloud setup, configuration might come from Spring Cloud Config Server. On AWS, common alternatives are:

- AWS Systems Manager Parameter Store
- AWS Secrets Manager
- Environment variables
- ECS task definitions
- EKS ConfigMaps and Secrets
- AWS AppConfig

Spring Boot already supports external configuration through `application.yml`, environment variables, command-line arguments, and profiles. Spring Cloud AWS extends this by allowing configuration to be loaded from AWS services.

Example use case:

```yaml
spring:
  config:
    import:
      - aws-parameterstore:/config/my-service/
      - aws-secretsmanager:/secrets/my-service/
```

Interview talking points:

- Parameter Store is good for non-secret configuration and simple hierarchical parameters.
- Secrets Manager is better for sensitive values, rotation, and managed secret lifecycle.
- Avoid hardcoding AWS credentials, secrets, queue URLs, and bucket names in code.
- Use profiles or environment-specific paths for `dev`, `qa`, `staging`, and `prod`.

## 4. Core Concept: Credentials and Region Resolution

Every AWS API call needs credentials and a region.

Spring Cloud AWS auto-configures AWS clients using the AWS SDK credential and region provider chains.

Common credential sources:

- Local development: AWS CLI profile, environment variables, SSO, or local credentials file
- EC2: instance profile
- ECS: task role
- EKS: IAM Roles for Service Accounts, often called IRSA
- CI/CD: OIDC federation or temporary STS credentials

Common region sources:

- `AWS_REGION` environment variable
- Spring configuration property
- EC2 or ECS metadata
- Explicit client configuration

Interview sound bite:

> In production, I do not want static access keys in the application. I prefer IAM roles: task roles for ECS, IRSA for EKS, instance profiles for EC2, and short-lived STS credentials for pipelines.

## 5. Core Concept: Service Discovery

Service discovery allows services to find each other dynamically instead of hardcoding hostnames and ports.

In classic Spring Cloud:

- Eureka Server is often used for service registry.
- Spring Cloud LoadBalancer can pick an instance.
- OpenFeign or `RestTemplate` can call services by logical name.

On AWS, service discovery is usually handled by infrastructure:

- ECS service discovery with AWS Cloud Map
- EKS Kubernetes Services and DNS
- Application Load Balancer or Network Load Balancer
- API Gateway for public or edge-facing APIs
- Private hosted zones in Route 53

Example:

Instead of calling:

```text
http://10.0.4.21:8080/orders
```

A service calls:

```text
http://orders-service.internal/orders
```

Interview talking points:

- In AWS, teams often avoid running Eureka because ECS, EKS, ALB, NLB, Cloud Map, and Route 53 already solve much of the discovery problem.
- Eureka may still appear in legacy Spring Cloud systems or non-AWS environments.
- For Kubernetes on EKS, native Kubernetes service discovery is usually preferred.

## 6. Core Concept: API Gateway and Routing

Spring Cloud Gateway provides request routing, filtering, authentication integration, rate limiting, and cross-cutting API behavior.

On AWS, similar responsibilities may be handled by:

- Amazon API Gateway
- Application Load Balancer
- CloudFront
- AWS WAF
- Spring Cloud Gateway inside ECS or EKS

Spring Cloud Gateway is useful when the team wants gateway behavior controlled in application code.

AWS API Gateway is useful when the team wants a managed edge service with features such as:

- Usage plans
- API keys
- Lambda integration
- Authorizers
- Throttling
- Edge-optimized endpoints

Interview sound bite:

> I would use AWS API Gateway for managed public API exposure and Spring Cloud Gateway when I need application-level routing logic, custom filters, or internal microservice gateway behavior.

## 7. Core Concept: Service-to-Service Communication

Spring applications commonly call other services using:

- `RestClient`
- `WebClient`
- OpenFeign
- gRPC
- Messaging through queues or topics

On AWS, synchronous service calls often go through:

- ALB
- NLB
- ECS service discovery
- EKS service DNS
- API Gateway

Asynchronous communication often uses:

- SQS
- SNS
- EventBridge
- Kinesis

Interview guidance:

- Use synchronous HTTP calls when the caller needs an immediate response.
- Use asynchronous messaging when work can happen later, needs retry, or needs loose coupling.
- Avoid long chains of synchronous service calls because they increase latency and failure propagation.

## 8. Core Concept: Messaging with SQS and SNS

SQS is a queue. SNS is a pub/sub topic.

SQS is commonly used for:

- Background jobs
- Retryable processing
- Decoupling producers and consumers
- Handling traffic spikes
- Dead-letter queues

SNS is commonly used for:

- Broadcasting events to multiple subscribers
- Fan-out architecture
- Sending one event to many SQS queues

Spring Cloud AWS provides listener-style integration for SQS.

Example:

```java
@SqsListener("order-created-queue")
public void handleOrderCreated(OrderCreatedEvent event) {
    // process the event
}
```

Typical architecture:

```text
Order Service -> SNS Topic -> SQS Queue -> Inventory Service
                         -> SQS Queue -> Email Service
                         -> SQS Queue -> Analytics Service
```

Interview talking points:

- Use SQS dead-letter queues for messages that repeatedly fail.
- Design message handlers to be idempotent because duplicate delivery can happen.
- For FIFO ordering, use SQS FIFO queues with message group IDs.
- Do not put large payloads directly in messages; store large data in S3 and send a reference.
- Use visibility timeout carefully so messages are not processed twice while still in progress.

## 9. Core Concept: Resilience and Circuit Breakers

Distributed systems fail in partial and unpredictable ways.

Common failure cases:

- A downstream service is slow
- An AWS API is throttling
- A network call times out
- A queue message fails repeatedly
- A database has hot partitions

Spring Cloud Circuit Breaker with Resilience4j is often used for:

- Circuit breakers
- Retries
- Timeouts
- Bulkheads
- Rate limiting

AWS also provides resilience features:

- SQS retries and dead-letter queues
- ALB health checks
- ECS and EKS self-healing
- Auto Scaling
- Multi-AZ deployments
- DynamoDB on-demand capacity or auto scaling
- RDS Multi-AZ

Interview sound bite:

> I treat resilience as layered. The application should use timeouts, retries, circuit breakers, and idempotency. AWS infrastructure should provide health checks, autoscaling, dead-letter queues, and multi-AZ availability.

Important retry warning:

- Do not retry every failure blindly.
- Retrying non-idempotent operations can create duplicate payments, duplicate orders, or inconsistent state.
- Use exponential backoff and jitter for AWS throttling scenarios.

## 10. Core Concept: Distributed Tracing and Observability

Observability helps answer:

- Is the service healthy?
- Where is latency coming from?
- Which dependency failed?
- Which request caused the error?
- How many messages are failing?

Spring Boot and Spring Cloud commonly integrate with:

- Micrometer
- OpenTelemetry
- Actuator
- Logs
- Metrics
- Traces

On AWS, observability often uses:

- CloudWatch Logs
- CloudWatch Metrics
- AWS X-Ray
- OpenSearch
- Managed Prometheus
- Managed Grafana

Spring Boot Actuator exposes endpoints such as:

```text
/actuator/health
/actuator/metrics
/actuator/prometheus
```

Interview talking points:

- Logs are for events and errors.
- Metrics are numeric time-series data.
- Traces show request flow across services.
- Health checks should distinguish liveness from readiness.
- Correlation IDs help connect logs across services.

## 11. Core Concept: Secrets Management

Secrets should not be stored in source code or plain config files.

Examples of secrets:

- Database passwords
- API keys
- OAuth client secrets
- Signing keys
- Third-party tokens

AWS options:

- AWS Secrets Manager for secrets with rotation and lifecycle management
- AWS Systems Manager Parameter Store secure strings for simpler secret storage
- IAM roles when possible to avoid secrets entirely

Spring Cloud AWS can load secrets into the Spring Environment, making them available as configuration properties.

Interview sound bite:

> For AWS-hosted Spring Boot services, I prefer IAM roles over static secrets. When a secret is unavoidable, I use Secrets Manager or secure Parameter Store values, not hardcoded properties.

## 12. Core Concept: Object Storage with S3

S3 is object storage, not a file system.

Common Spring Boot use cases:

- Uploading user files
- Storing reports
- Reading batch input files
- Serving static assets
- Archiving logs or exports

Spring Cloud AWS can expose S3 objects through Spring-style resource abstractions and auto-configured clients.

Example ideas:

```java
// Conceptual usage
Resource resource = resourceLoader.getResource("s3://my-bucket/reports/report.csv");
```

Interview talking points:

- S3 is highly durable object storage.
- Use pre-signed URLs for controlled upload/download access.
- Use bucket policies and IAM least privilege.
- Use lifecycle policies to move old objects to cheaper storage classes.
- Use event notifications to trigger downstream processing.

## 13. Core Concept: Data with DynamoDB

DynamoDB is a managed NoSQL key-value and document database.

Good use cases:

- High-scale key-value lookup
- Event metadata
- Session state
- Shopping carts
- Idempotency keys
- Fast access by known partition key

Less ideal use cases:

- Complex joins
- Ad hoc relational queries
- Heavy cross-table transactions
- Unknown access patterns

Spring Cloud AWS can simplify DynamoDB client configuration and Spring-style operations.

Interview talking points:

- DynamoDB table design starts with access patterns.
- Partition key design is critical for avoiding hot partitions.
- Use conditional writes for idempotency and concurrency control.
- Use TTL for expiring temporary data.
- Use GSIs carefully because they affect cost and write behavior.

## 14. Deployment Patterns on AWS

Common deployment targets:

- Elastic Beanstalk for simpler app deployment
- ECS with Fargate for containerized services without managing servers
- EKS for Kubernetes-based platforms
- EC2 for more manual control
- Lambda for serverless workloads

Spring Boot services are often packaged as:

- Executable JARs
- Docker containers
- Native images in some performance-sensitive cases

Typical ECS deployment:

```text
Spring Boot container
-> ECS Service
-> Fargate tasks
-> Application Load Balancer
-> CloudWatch Logs
-> IAM task role
-> SQS/SNS/S3/DynamoDB/etc.
```

Typical EKS deployment:

```text
Spring Boot container
-> Kubernetes Deployment
-> Kubernetes Service
-> Ingress or ALB Controller
-> IRSA for AWS permissions
-> CloudWatch/OpenTelemetry observability
```

## 15. Dependency Management

Use a BOM to keep Spring Cloud AWS module versions aligned.

Maven example:

```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>io.awspring.cloud</groupId>
            <artifactId>spring-cloud-aws-dependencies</artifactId>
            <version>${spring-cloud-aws.version}</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

Starter example:

```xml
<dependency>
    <groupId>io.awspring.cloud</groupId>
    <artifactId>spring-cloud-aws-starter</artifactId>
</dependency>
```

Service-specific starters may include:

```xml
<dependency>
    <groupId>io.awspring.cloud</groupId>
    <artifactId>spring-cloud-aws-starter-sqs</artifactId>
</dependency>

<dependency>
    <groupId>io.awspring.cloud</groupId>
    <artifactId>spring-cloud-aws-starter-s3</artifactId>
</dependency>
```

Interview talking points:

- Use compatible Spring Boot, Spring Cloud, Spring Cloud AWS, and AWS SDK versions.
- Prefer BOMs instead of manually pinning every transitive dependency.
- Match the documentation version to your Spring Boot version.

## 16. Common Interview Questions and Strong Answers

### What problem does Spring Cloud solve?

Spring Cloud provides patterns and tools for distributed systems: configuration, discovery, routing, load balancing, fault tolerance, messaging, and observability. It helps Spring Boot services operate as part of a larger microservice architecture.

### How does Spring Cloud AWS help?

It integrates AWS managed services with Spring Boot idioms. Instead of manually wiring AWS SDK clients everywhere, Spring Cloud AWS can auto-configure clients, credentials, regions, SQS listeners, S3 resource access, Parameter Store configuration, Secrets Manager configuration, and other AWS integrations.

### Would you use Eureka on AWS?

Not always. In AWS, service discovery can often be handled by ECS service discovery, AWS Cloud Map, EKS DNS, Route 53, or load balancers. Eureka may still be used in legacy systems, hybrid systems, or when a team wants Spring-managed discovery independent of AWS infrastructure.

### How do you secure AWS credentials in a Spring Boot app?

I avoid static credentials in code. On ECS I use task roles. On EKS I use IRSA. On EC2 I use instance profiles. For local development, I use AWS profiles or SSO. If secrets are needed, I use Secrets Manager or Parameter Store and apply least-privilege IAM policies.

### SQS vs SNS?

SQS is a queue used for point-to-point asynchronous processing. SNS is a pub/sub topic used to fan out messages to multiple subscribers. A common pattern is publishing one event to SNS and having multiple SQS queues subscribed to it.

### How do you handle duplicate SQS messages?

I design consumers to be idempotent. I might store a processed event ID, use DynamoDB conditional writes, or make the operation naturally safe to repeat. SQS standard queues can deliver duplicates, so the application must be prepared.

### How do you handle failure in distributed Spring services?

I use timeouts, retries with backoff and jitter, circuit breakers, idempotency, dead-letter queues, health checks, and monitoring. I avoid infinite retries and make sure retries are safe for the operation.

### Parameter Store vs Secrets Manager?

Parameter Store is good for configuration values and simple secure strings. Secrets Manager is better for sensitive secrets that require rotation, auditing, and lifecycle management. For production database credentials, Secrets Manager is often the stronger choice.

### What is the role of Spring Boot Actuator in AWS?

Actuator exposes health, metrics, and operational endpoints. AWS load balancers, ECS, EKS, CloudWatch, Prometheus, or other monitoring tools can use those endpoints to check service health and collect metrics.

## 17. Quick AWS Mapping Table

| Spring Cloud Concept | AWS Service or Pattern |
| --- | --- |
| External configuration | Parameter Store, Secrets Manager, AppConfig |
| Service discovery | Cloud Map, ECS discovery, EKS DNS, Route 53 |
| API gateway | API Gateway, ALB, Spring Cloud Gateway |
| Load balancing | ALB, NLB, Kubernetes Service, Spring Cloud LoadBalancer |
| Messaging | SQS, SNS, EventBridge |
| Streaming | Kinesis, MSK |
| Secrets | Secrets Manager, Parameter Store secure strings, IAM roles |
| Metrics | CloudWatch, Micrometer, Prometheus |
| Tracing | AWS X-Ray, OpenTelemetry |
| Object storage | S3 |
| NoSQL data | DynamoDB |
| Deployment | ECS, EKS, Elastic Beanstalk, EC2, Lambda |

## 18. Best Practices

- Use IAM roles instead of long-lived access keys.
- Apply least-privilege IAM policies per service.
- Keep AWS resource names configurable.
- Use Spring profiles for environment differences.
- Prefer managed AWS services instead of self-hosting infrastructure when they meet the requirement.
- Design SQS consumers to be idempotent.
- Use dead-letter queues for failed async processing.
- Use timeouts on all network calls.
- Use retries only when the operation is safe to retry.
- Export logs, metrics, and traces for production visibility.
- Use BOMs to avoid dependency version mismatch.
- Match Spring Boot, Spring Cloud, Spring Cloud AWS, and AWS SDK versions carefully.

## 19. Common Mistakes

- Hardcoding AWS access keys in `application.yml`.
- Treating S3 like a normal file system.
- Using SQS without a dead-letter queue.
- Assuming SQS standard queues deliver each message exactly once.
- Retrying non-idempotent operations without safeguards.
- Running Eureka on AWS when native discovery would be simpler.
- Putting secrets in plain environment variables without a rotation plan.
- Giving applications overly broad IAM permissions.
- Ignoring visibility timeout and message processing duration.
- Forgetting that DynamoDB design depends on access patterns.

## 20. Short Final Review

For interviews, remember this core framing:

Spring Cloud gives Spring Boot applications distributed-system patterns. AWS gives managed infrastructure services that implement many of those patterns. Spring Cloud AWS connects the two by making AWS services feel like normal Spring components.

Strong candidates can explain not only how to add a dependency, but also when to use AWS-managed capabilities instead of self-hosted Spring Cloud infrastructure. The most important topics are configuration, credentials, service discovery, messaging, resilience, observability, secrets, and deployment.

## Sources

- Spring Cloud reference documentation: https://docs.spring.io/spring-cloud/docs/current/reference/html/
- Spring Cloud AWS modern documentation: https://docs.awspring.io/spring-cloud-aws/docs/4.0.2/reference/html/index.html
- Historical Spring Cloud AWS documentation and migration note: https://docs.spring.io/spring-cloud-aws/docs/current/2.2.6.RELEASE/reference/html/README.html
