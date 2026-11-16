# **Code Review – Concepts, Principles, Rules & Terminology**

Code review is a systematic process of examining code written by another developer (or yourself) before merging it into the main codebase.

The objective is not just to find bugs. It is to ensure that code is **correct, maintainable, readable, secure, testable, performant, and consistent with project architecture.**

Below is a comprehensive guide covering the terminology, engineering principles, review rules, and practices used in professional code reviews and senior-level engineering interviews.

# **1. Core code review principles**

These are the fundamental principles that guide every code review.

| Principle                    | Meaning                                                                       |
| ---------------------------- | ----------------------------------------------------------------------------- |
| Correctness                  | Code produces the expected result.                                            |
| Simplicity                   | Prefer the simplest solution that solves the problem.                         |
| Readability                  | Code should be easy for another developer to understand.                      |
| Maintainability              | Code should be easy to modify in the future.                                  |
| Consistency                  | Follow existing project conventions and patterns.                             |
| Reusability                  | Avoid unnecessary duplication; reuse appropriate abstractions.                |
| Separation of Concerns (SoC) | Each component should have a clearly defined responsibility.                  |
| Testability                  | Code should be easy to test independently.                                    |
| Reliability                  | Code should behave predictably, including during failures.                    |
| Security                     | Prevent vulnerabilities and unauthorized access.                              |
| Performance                  | Avoid unnecessary computation, memory usage, and database calls.              |
| Extensibility                | New functionality should be added without excessive changes.                  |
| Observability                | Errors and important operations should be traceable through logs and metrics. |
| Backward Compatibility       | Changes should not unexpectedly break existing consumers.                     |
| Least Surprise               | Code should behave as other developers reasonably expect.                     |

# **2. SOLID principles**

SOLID is one of the most important sets of principles used during object-oriented code reviews.

## **S – Single Responsibility Principle (SRP)**

A class or module should have one primary responsibility and one reason to change.

**Bad example:**

```
class UserService:    def create_user(self):        pass    def send_email(self):        pass    def generate_invoice(self):        pass
```

This class handles unrelated responsibilities.

**Better:**

```
class UserService:    def create_user(self):        passclass EmailService:    def send_email(self):        passclass InvoiceService:    def generate_invoice(self):        pass
```

**Code review questions:**

- Does this class do too many things?
- Are unrelated responsibilities mixed?
- Can responsibilities be separated?

## **O – Open/Closed Principle (OCP)**

Software entities should be open for extension but closed for modification.

You should be able to introduce new behavior without repeatedly modifying stable existing code.

**Bad:**

```
def calculate_discount(customer_type):    if customer_type == "regular":        return 5    elif customer_type == "premium":        return 10
```

Adding new customer types requires changing this function.

**Better:** Use a strategy or polymorphism-based approach.

```
class DiscountStrategy:    def calculate(self):        raise NotImplementedErrorclass PremiumDiscount(DiscountStrategy):    def calculate(self):        return 10
```

**Review question:** Will adding another business rule require modifying multiple existing components?

## **L – Liskov Substitution Principle (LSP)**

A child class should be usable wherever its parent class is expected without breaking behavior.

Example: If a function expects a `Bird` that can fly, a `Penguin` should not inherit that flying contract.

**Review questions:**

- Does a subclass violate parent-class expectations?
- Does it unexpectedly raise unsupported-operation errors?
- Does it strengthen preconditions or weaken postconditions?

## **I – Interface Segregation Principle (ISP)**

Do not force a class to implement methods it does not need.

Prefer smaller, focused interfaces over large interfaces.

**Bad:** A read-only repository is forced to implement create, update, and delete methods.

**Better:** Separate read and write interfaces.

## **D – Dependency Inversion Principle (DIP)**

High-level business logic should depend on abstractions, not concrete implementations.

**Bad:**

```
class OrderService:    def __init__(self):        self.db = PostgreSQLDatabase()
```

**Better:**

```
class OrderService:    def __init__(self, repository):        self.repository = repository
```

The implementation can now be PostgreSQL, MongoDB, or an in-memory repository.

# **3. Clean Code principles and rules**

These are frequently used when reviewing everyday code.

| Rule / Principle                 | Explanation                                                                                     |
| -------------------------------- | ----------------------------------------------------------------------------------------------- |
| DRY – Don't Repeat Yourself      | Avoid duplicating business logic.                                                               |
| KISS – Keep It Simple, Stupid    | Avoid unnecessary complexity.                                                                   |
| YAGNI – You Aren't Gonna Need It | Don't build functionality without a current requirement.                                        |
| WET – Write Everything Twice     | A reminder that some duplication is acceptable before introducing abstractions.                 |
| Principle of Least Astonishment  | Code should behave predictably.                                                                 |
| Boy Scout Rule                   | Leave code cleaner than you found it.                                                           |
| Composition over Inheritance     | Prefer combining small components over deep inheritance.                                        |
| Encapsulation                    | Hide internal implementation details.                                                           |
| Information Hiding               | Expose only what consumers need.                                                                |
| High Cohesion                    | Related functionality should stay together.                                                     |
| Low Coupling                     | Components should have minimal dependencies.                                                    |
| Explicit over Implicit           | Make behavior and dependencies clear.                                                           |
| Fail Fast                        | Detect invalid conditions as early as possible.                                                 |
| Defensive Programming            | Validate assumptions and handle unexpected inputs.                                              |
| Command-Query Separation (CQS)   | A method should either change state or return information, not both.                            |
| Tell, Don't Ask                  | Give objects commands rather than extracting their internal state to make decisions externally. |

## **Important code-level review rules**

**1. Avoid magic numbers**

```
# Badif age >= 18:    pass# BetterMINIMUM_AGE = 18if age >= MINIMUM_AGE:    pass
```

**2. Meaningful naming**

```
# Baddef calc(x, y):    return x * y# Betterdef calculate_total_price(quantity, unit_price):    return quantity * unit_price
```

**3. Keep functions focused**

A function should ideally perform one logical task.

**4. Avoid deep nesting**

Prefer guard clauses and early returns.

```
# Betterdef process_user(user):    if not user:        return None    if not user.is_active:        return None    return process(user)
```

**5. Avoid boolean blindness**

Instead of:

```
create_user(data, True, False)
```

Prefer:

```
create_user(    data,    send_welcome_email=True,    is_admin=False)
```

**6. Avoid mutable default arguments in Python**

```
# Baddef add_item(item, items=[]):    items.append(item)    return items# Betterdef add_item(item, items=None):    if items is None:        items = []    items.append(item)    return items
```

# **4. Design principles and architectural rules**

These principles help reviewers evaluate whether code fits the overall system architecture.

| Principle / Term                            | Explanation                                                                                |
| ------------------------------------------- | ------------------------------------------------------------------------------------------ |
| Separation of Concerns (SoC)                | Separate business logic, data access, validation, and presentation.                        |
| Layered Architecture                        | Organize code into presentation, service, repository, and database layers.                 |
| Dependency Injection (DI)                   | Provide dependencies externally instead of creating them inside classes.                   |
| Inversion of Control (IoC)                  | Transfer control of object creation and dependency management to a framework or container. |
| Repository Pattern                          | Abstract database operations behind a repository.                                          |
| Service Layer Pattern                       | Keep business operations in a dedicated service layer.                                     |
| Factory Pattern                             | Centralize object creation logic.                                                          |
| Strategy Pattern                            | Switch between interchangeable algorithms at runtime.                                      |
| Adapter Pattern                             | Make incompatible interfaces work together.                                                |
| Facade Pattern                              | Provide a simplified interface over a complex subsystem.                                   |
| Decorator Pattern                           | Add behavior without modifying the original component.                                     |
| Observer Pattern                            | Notify subscribers when an event occurs.                                                   |
| Builder Pattern                             | Construct complex objects step by step.                                                    |
| Ports and Adapters (Hexagonal Architecture) | Keep business logic independent of external systems.                                       |
| Clean Architecture                          | Organize dependencies so business rules remain independent of frameworks.                  |
| Domain-Driven Design (DDD)                  | Structure software around business domains and domain models.                              |
| Bounded Context                             | Define boundaries within which a domain model has a consistent meaning.                    |
| Anti-Corruption Layer                       | Prevent external systems' models from leaking into your domain model.                      |

**Important review rule: Dependency Rule**

Dependencies should point inward toward business logic, not outward toward frameworks or infrastructure.

Example:

```
API Controller
     |
     v
Service Layer
     |
     v
Repository Interface
     |
     v
Database Implementation
```

The service should depend on the repository abstraction, not directly on database implementation details.

# **5. Code smells and anti-patterns**

Code smells are warning signs that code may have structural or maintainability problems. They don't always mean there is a bug.

| Code Smell / Anti-pattern | Meaning                                                             |
| ------------------------- | ------------------------------------------------------------------- |
| God Class                 | One class handles too many responsibilities.                        |
| God Object                | One object controls too much of the application.                    |
| Long Method               | A function contains too much logic.                                 |
| Large Class               | A class has too many fields and methods.                            |
| Long Parameter List       | A function requires too many arguments.                             |
| Feature Envy              | A method uses another class's data more than its own.               |
| Shotgun Surgery           | One change requires modifications across many classes.              |
| Divergent Change          | One class changes for many unrelated reasons.                       |
| Duplicate Code            | Same logic appears in multiple places.                              |
| Dead Code                 | Code that is never executed or used.                                |
| Speculative Generality    | Unnecessary abstractions built for hypothetical requirements.       |
| Primitive Obsession       | Using primitive types instead of meaningful domain objects.         |
| Data Clumps               | The same group of parameters repeatedly appears together.           |
| Circular Dependency       | Two or more modules depend on each other cyclically.                |
| Callback Hell             | Excessively nested asynchronous callbacks.                          |
| Spaghetti Code            | Unstructured, difficult-to-follow control flow.                     |
| Lava Flow                 | Old, poorly understood code that remains in the system.             |
| Golden Hammer             | Using one familiar technology or pattern for every problem.         |
| Copy-Paste Programming    | Duplicating code rather than extracting appropriate reusable logic. |
| Overengineering           | Adding unnecessary abstraction or complexity.                       |
| Premature Optimization    | Optimizing before identifying a real performance problem.           |
| Leaky Abstraction         | Internal implementation details leak through an abstraction.        |
| Tight Coupling            | Components are heavily dependent on one another.                    |

## **Refactoring techniques reviewers should recognize**

| Refactoring Technique                         | Purpose                                                              |
| --------------------------------------------- | -------------------------------------------------------------------- |
| Extract Method                                | Move a block of code into a separate function.                       |
| Extract Class                                 | Move responsibilities into a new class.                              |
| Inline Method                                 | Remove unnecessary method indirection.                               |
| Rename Variable                               | Improve clarity through better naming.                               |
| Replace Magic Number with Constant            | Give unnamed values meaningful names.                                |
| Introduce Parameter Object                    | Group related parameters into one object.                            |
| Replace Conditional with Polymorphism         | Replace complex type-based conditions with polymorphic behavior.     |
| Encapsulate Field                             | Control access to internal state.                                    |
| Move Method                                   | Place a method in the class where its responsibility belongs.        |
| Replace Nested Conditional with Guard Clauses | Reduce nested branches.                                              |
| Split Phase                                   | Separate different processing stages.                                |
| Strangler Pattern                             | Gradually replace legacy functionality without a risky full rewrite. |

# **6. Correctness and logic review**

This is where reviewers check whether the code actually works for all relevant situations.

| Term / Rule          | What to check                                                    |
| -------------------- | ---------------------------------------------------------------- |
| Edge Cases           | Empty input, null values, zero, negative values, maximum values. |
| Boundary Conditions  | Values exactly at minimum and maximum limits.                    |
| Input Validation     | Verify input types, formats, and constraints.                    |
| Preconditions        | Conditions that must be true before execution.                   |
| Postconditions       | Conditions guaranteed after successful execution.                |
| Invariants           | Conditions that must always remain true.                         |
| Idempotency          | Repeated execution produces the same intended result.            |
| Determinism          | Same input and state produce the same output.                    |
| Atomicity            | Operation happens completely or not at all.                      |
| Consistency          | Operations preserve defined business and data rules.             |
| State Transition     | Validate allowed changes between application states.             |
| Null Safety          | Prevent unexpected null-related failures.                        |
| Type Safety          | Prevent invalid type usage.                                      |
| Overflow / Underflow | Check numeric limits and arithmetic behavior.                    |
| Off-by-One Error     | Check incorrect loop or array boundaries.                        |
| Race Condition       | Check concurrent operations that depend on timing.               |
| TOCTOU               | Check whether a condition changes between validation and use.    |
| Error Propagation    | Ensure failures are not silently swallowed.                      |

**Three particularly important concepts:**

- **Precondition:** What must be true before a function runs?
- **Postcondition:** What must be true after it finishes?
- **Invariant:** What must always remain true?

Example:

```
def withdraw(account, amount):    # Precondition    assert amount > 0    # Business invariant    if account.balance < amount:        raise ValueError("Insufficient balance")    account.balance -= amount    # Postcondition    assert account.balance >= 0
```

Assertions here illustrate contracts; production input validation and transactional safeguards should not rely on assertions alone.

# **7. Testing principles and terminology**

A reviewer should examine not just whether tests exist, but whether they provide meaningful coverage.

| Term                     | Explanation                                                          |
| ------------------------ | -------------------------------------------------------------------- |
| Unit Testing             | Test individual functions or components.                             |
| Integration Testing      | Test interactions between multiple components.                       |
| End-to-End (E2E) Testing | Test complete application workflows.                                 |
| Regression Testing       | Ensure existing functionality still works after changes.             |
| Smoke Testing            | Verify critical functionality works at a basic level.                |
| Sanity Testing           | Quickly verify a specific change or fix.                             |
| Acceptance Testing       | Verify requirements from the user's or business perspective.         |
| Contract Testing         | Verify agreements between service consumers and providers.           |
| Snapshot Testing         | Compare output against a previously recorded snapshot.               |
| Property-Based Testing   | Test general properties across generated inputs.                     |
| Mutation Testing         | Intentionally introduce code changes to evaluate test effectiveness. |
| Parameterized Testing    | Run the same test with different inputs.                             |
| Test Fixture             | Set up reusable test data and environments.                          |
| Mocking                  | Replace dependencies with controlled fake behavior.                  |
| Stubbing                 | Provide predefined responses from dependencies.                      |
| Faking                   | Provide a lightweight working implementation of a dependency.        |
| Spying                   | Record calls made to a dependency.                                   |
| Test Isolation           | Ensure tests do not interfere with each other.                       |
| Flaky Test               | A test that inconsistently passes or fails.                          |
| Test Coverage            | Measure which code is executed by tests.                             |
| Branch Coverage          | Measure whether decision branches are exercised.                     |
| Condition Coverage       | Check individual Boolean conditions.                                 |
| Test Pyramid             | More unit tests, fewer integration tests, and fewer E2E tests.       |
| AAA Pattern              | Arrange, Act, Assert.                                                |
| Given-When-Then          | Structure tests around context, action, and expected outcome.        |

## **Testing rules to apply during code review**

1. Every bug fix should ideally include a regression test.
2. Test positive, negative, and boundary scenarios.
3. Avoid tests that depend on execution order.
4. Avoid testing implementation details unnecessarily.
5. Mock external dependencies, not everything.
6. Verify error paths, not just successful execution.
7. Avoid flaky tests and uncontrolled external dependencies.
8. Test business behavior rather than merely checking that a method was called.

# **8. Security review principles**

Security is a critical part of professional code review.

| Term / Principle      | Explanation                                                            |
| --------------------- | ---------------------------------------------------------------------- |
| OWASP Top 10          | Common categories of web application security risks.                   |
| Least Privilege       | Give users and services only necessary permissions.                    |
| Defense in Depth      | Apply multiple independent security controls.                          |
| Zero Trust            | Never automatically trust a user, device, or service.                  |
| Secure by Default     | Default configurations should be secure.                               |
| Fail Securely         | Failure should not expose data or grant access.                        |
| Input Sanitization    | Remove or neutralize unsafe input where appropriate.                   |
| Output Encoding       | Encode output according to its destination context.                    |
| SQL Injection         | Malicious SQL inserted through unsafe query construction.              |
| XSS                   | Injecting malicious scripts into web pages.                            |
| CSRF                  | Tricking authenticated users into making unwanted requests.            |
| SSRF                  | Exploiting server-side requests to access unintended resources.        |
| IDOR                  | Accessing another user's resources through insecure object references. |
| Broken Access Control | Failure to enforce authorization correctly.                            |
| Authentication        | Verifying who a user is.                                               |
| Authorization         | Verifying what a user is allowed to do.                                |
| RBAC                  | Access based on roles.                                                 |
| ABAC                  | Access based on attributes and policies.                               |
| Secret Management     | Safely storing API keys, credentials, and tokens.                      |
| Encryption at Rest    | Protecting stored data.                                                |
| Encryption in Transit | Protecting data during transmission.                                   |
| Audit Logging         | Recording security-relevant actions.                                   |
| Rate Limiting         | Restricting request frequency.                                         |
| Replay Attack         | Reusing a previously captured valid request.                           |
| Security Headers      | Browser-facing headers that enforce security protections.              |
| Dependency Scanning   | Detecting vulnerable third-party packages.                             |
| SAST                  | Static Application Security Testing.                                   |
| DAST                  | Dynamic Application Security Testing.                                  |
| SBOM                  | Software Bill of Materials; inventory of software dependencies.        |

**Security review questions:**

- Are secrets hardcoded?
- Is user input validated?
- Are SQL queries parameterized?
- Is authorization checked at the resource level?
- Are sensitive fields exposed in API responses?
- Are errors leaking internal details?
- Are file uploads restricted and validated?
- Are third-party dependencies vulnerable?

# **9. Performance and scalability review**

Reviewers should identify unnecessary resource consumption and potential bottlenecks.

| Term / Concept     | Explanation                                                       |
| ------------------ | ----------------------------------------------------------------- |
| Time Complexity    | How execution time grows with input size.                         |
| Space Complexity   | How memory requirements grow with input size.                     |
| Big O Notation     | Describes algorithmic growth rate.                                |
| N+1 Query Problem  | One query followed by unnecessary queries for each record.        |
| Eager Loading      | Fetch related data upfront.                                       |
| Lazy Loading       | Fetch related data only when needed.                              |
| Database Indexing  | Speed up data retrieval using indexes.                            |
| Query Optimization | Improve database query execution.                                 |
| Connection Pooling | Reuse database connections.                                       |
| Caching            | Store frequently accessed results for reuse.                      |
| Cache Invalidation | Remove or update stale cached data.                               |
| Pagination         | Retrieve large datasets in smaller portions.                      |
| Batch Processing   | Process multiple records together.                                |
| Vectorization      | Process multiple values using optimized operations.               |
| Memoization        | Cache function results based on inputs.                           |
| Lazy Evaluation    | Delay computation until results are needed.                       |
| Streaming          | Process data incrementally instead of loading everything.         |
| Backpressure       | Control producers when consumers cannot keep up.                  |
| Horizontal Scaling | Add more application instances.                                   |
| Vertical Scaling   | Increase resources of an existing machine.                        |
| Load Balancing     | Distribute requests across instances.                             |
| Concurrency        | Manage multiple tasks making progress during overlapping periods. |
| Parallelism        | Execute multiple tasks simultaneously.                            |
| Async I/O          | Avoid blocking while waiting for I/O operations.                  |
| Thread Safety      | Ensure safe access to shared data across threads.                 |
| Memory Leak        | Memory remains allocated when no longer needed.                   |
| Resource Leak      | Resources such as connections or file handles are not released.   |

## **Important database-specific review rules**

| Rule                   | Explanation                                                                 |
| ---------------------- | --------------------------------------------------------------------------- |
| Avoid SELECT \*        | Retrieve only required columns.                                             |
| Parameterized Queries  | Prevent SQL injection and improve query safety.                             |
| Transaction Boundaries | Group related database operations correctly.                                |
| Commit vs Flush        | Understand when changes are persisted and when transactions become durable. |
| Rollback Handling      | Revert failed transactions appropriately.                                   |
| Optimistic Locking     | Detect concurrent modifications using version checks.                       |
| Pessimistic Locking    | Lock records to prevent conflicting operations.                             |
| Deadlock Prevention    | Avoid conflicting lock acquisition patterns.                                |
| Foreign Key Integrity  | Maintain valid relationships between records.                               |
| Unique Constraints     | Prevent duplicate values where required.                                    |
| NOT NULL Constraints   | Enforce required data.                                                      |
| Migration Safety       | Ensure schema changes are safe to deploy.                                   |
| Soft Delete            | Mark records deleted instead of physically removing them.                   |
| Connection Lifecycle   | Properly acquire and release database connections.                          |

# **10. API and backend code review**

Particularly important when reviewing FastAPI, Flask, Django, or microservices.

| Term / Rule             | Explanation                                                               |
| ----------------------- | ------------------------------------------------------------------------- |
| REST Principles         | Resource-oriented API design using HTTP semantics.                        |
| HTTP Semantics          | Use methods and status codes correctly.                                   |
| API Contract            | Define expected request and response structures.                          |
| Schema Validation       | Validate incoming and outgoing data structures.                           |
| DTO                     | Data Transfer Object for moving data between layers.                      |
| Serialization           | Convert objects into transferable formats.                                |
| Deserialization         | Convert external data into application objects.                           |
| API Versioning          | Manage changes without unexpectedly breaking consumers.                   |
| Idempotent HTTP Methods | Methods such as GET and PUT should have appropriate idempotent behavior.  |
| Retry Safety            | Ensure retrying requests does not create unintended duplicate operations. |
| Timeout Handling        | Set reasonable limits for external operations.                            |
| Retry with Backoff      | Retry transient failures with increasing delays.                          |
| Circuit Breaker         | Temporarily stop calling an unhealthy dependency.                         |
| Bulkhead Pattern        | Isolate resources to prevent cascading failures.                          |
| Correlation ID          | Track a request across multiple services.                                 |
| Request Validation      | Reject invalid request data.                                              |
| Response Filtering      | Return only authorized and necessary fields.                              |
| Middleware              | Execute common logic around request handling.                             |
| Exception Handler       | Centralize application error handling.                                    |
| Rate Limiting           | Protect APIs against excessive requests.                                  |
| Health Check            | Expose application health information.                                    |
| Graceful Shutdown       | Finish or safely terminate ongoing work before stopping.                  |

## **HTTP status codes reviewers should recognize**

| Status | Meaning                        |
| ------ | ------------------------------ |
| 200    | OK                             |
| 201    | Created                        |
| 202    | Accepted for processing        |
| 204    | No Content                     |
| 400    | Bad Request                    |
| 401    | Unauthorized / unauthenticated |
| 403    | Forbidden                      |
| 404    | Not Found                      |
| 409    | Conflict                       |
| 422    | Unprocessable Content          |
| 429    | Too Many Requests              |
| 500    | Internal Server Error          |
| 502    | Bad Gateway                    |
| 503    | Service Unavailable            |
| 504    | Gateway Timeout                |

# **11. Error handling and logging principles**

| Term                      | Explanation                                      |
| ------------------------- | ------------------------------------------------ |
| Exception Handling        | Handle unexpected execution failures.            |
| Exception Chaining        | Preserve the original exception context.         |
| Custom Exceptions         | Define application-specific failure types.       |
| Fail Fast                 | Detect invalid states early.                     |
| Graceful Degradation      | Continue operating with reduced functionality.   |
| Error Propagation         | Pass failures to the appropriate handling layer. |
| Error Masking             | Accidentally hiding the original error.          |
| Structured Logging        | Store logs in machine-readable formats.          |
| Log Levels                | DEBUG, INFO, WARNING, ERROR, CRITICAL.           |
| Contextual Logging        | Include request IDs and relevant context.        |
| PII Masking               | Hide sensitive personal information in logs.     |
| Distributed Tracing       | Follow requests across multiple services.        |
| Metrics                   | Track numerical system measurements.             |
| Alerting                  | Notify teams when defined conditions occur.      |
| Root Cause Analysis (RCA) | Identify the underlying cause of a failure.      |
| Error Budget              | Allowed unreliability within an SLO.             |

**Important rule:** Never silently swallow exceptions.

Bad:

```
try:    process_payment()except Exception:    pass
```

Better:

```
try:    process_payment()except PaymentError:    logger.exception("Payment processing failed")    raise
```

Catch specific exceptions wherever possible, and handle them at the layer that can meaningfully respond.

# **12. Git, pull request and review workflow terminology**

These are commonly used in actual engineering teams.

| Term                      | Explanation                                                       |
| ------------------------- | ----------------------------------------------------------------- |
| Pull Request (PR)         | Request to merge code changes into another branch.                |
| Merge Request (MR)        | GitLab's equivalent of a pull request.                            |
| Code Owner                | Person/team responsible for reviewing particular files.           |
| Reviewer                  | Developer responsible for examining proposed changes.             |
| Author                    | Developer who submitted the changes.                              |
| Approve                   | Reviewer accepts the proposed changes.                            |
| Request Changes           | Reviewer identifies issues that must be addressed.                |
| Comment                   | Feedback or question about the code.                              |
| Inline Comment            | Feedback attached to a particular line.                           |
| Nitpick                   | Minor style or readability suggestion.                            |
| Blocking Comment          | Issue that must be resolved before merging.                       |
| Non-blocking Comment      | Suggestion that does not prevent merging.                         |
| LGTM                      | Looks Good To Me.                                                 |
| ACK                       | Acknowledgement or agreement.                                     |
| NACK                      | Explicit disagreement or rejection.                               |
| Squash Merge              | Combine multiple commits into one.                                |
| Rebase                    | Reapply commits on top of another base commit.                    |
| Merge Conflict            | Incompatible changes between branches.                            |
| Commit Message Convention | Standard format for describing changes.                           |
| Conventional Commits      | Structured commit naming, e.g. `feat:`, `fix:`, `refactor:`.      |
| CI/CD                     | Automated integration, testing, and delivery/deployment pipeline. |
| Branch Protection         | Rules that restrict merging and direct changes.                   |
| Required Status Checks    | Automated checks that must pass before merging.                   |
| Stale Review              | Approval invalidated by subsequent changes.                       |

## **PR review comment classification**

A useful convention is to classify comments by severity.

| Priority      | Meaning                                        | Example                         |
| ------------- | ---------------------------------------------- | ------------------------------- |
| P0 – Critical | Immediate security or system-wide failure      | Exposing production credentials |
| P1 – High     | Major bug or broken functionality              | Incorrect payment calculation   |
| P2 – Medium   | Important correctness or maintainability issue | Missing validation              |
| P3 – Low      | Minor improvement                              | Unclear naming                  |
| Nit           | Cosmetic suggestion                            | Formatting preference           |

Note: P0–P3 is a common convention, not a universal standard. Teams may define their own severity system.

## **Examples of good review comments**

**Bad comment:**

"Wrong implementation. Change this."

**Better comment:**

"Could we use a parameterized query here? Direct string interpolation can expose this endpoint to SQL injection."

Another example:

"Can we extract this validation into a separate function? The same validation is already used in two other endpoints, so centralizing it would reduce duplication."

A good review comment should explain:

1. What is the problem?
2. Why does it matter?
3. What improvement is recommended?
4. Is it blocking or optional?

# **13. CI/CD and automated code quality checks**

Not every review issue needs to be found manually. Professional teams automate many checks.

| Tool / Concept      | Purpose                                      |
| ------------------- | -------------------------------------------- |
| Linter              | Detect style violations and suspicious code. |
| Formatter           | Automatically format code consistently.      |
| Static Analysis     | Analyze code without executing it.           |
| Type Checker        | Detect incompatible types.                   |
| SAST                | Detect security issues in source code.       |
| Dependency Scanner  | Detect vulnerable dependencies.              |
| Test Runner         | Execute automated tests.                     |
| Coverage Reporter   | Report test coverage.                        |
| Complexity Analyzer | Measure code complexity.                     |
| Pre-commit Hooks    | Run checks before creating commits.          |
| Build Validation    | Verify code can be built.                    |
| CI Pipeline         | Automatically run checks on changes.         |
| Quality Gate        | Conditions that must pass before merging.    |
| SonarQube           | Code quality and static analysis platform.   |
| Ruff                | Python linting and formatting.               |
| Black               | Python code formatter.                       |
| MyPy                | Python static type checker.                  |
| Pyright             | Python type checker.                         |
| Bandit              | Python security issue scanner.               |
| Semgrep             | Rule-based static code analysis.             |
| Trivy               | Vulnerability and configuration scanning.    |

# **14. Advanced engineering principles**

These are particularly useful for senior developer and architecture-level code reviews.

| Principle / Concept     | Explanation                                                                                  |
| ----------------------- | -------------------------------------------------------------------------------------------- |
| CAP Theorem             | Distributed systems trade-offs involving consistency, availability, and partition tolerance. |
| Eventual Consistency    | Replicas may temporarily differ but converge later.                                          |
| Strong Consistency      | Reads reflect the latest committed write according to the consistency model.                 |
| ACID                    | Atomicity, Consistency, Isolation, Durability.                                               |
| BASE                    | Basically Available, Soft State, Eventual Consistency.                                       |
| Saga Pattern            | Coordinate distributed transactions through local transactions and compensations.            |
| Outbox Pattern          | Reliably publish events alongside database transactions.                                     |
| CQRS                    | Separate command operations from query operations.                                           |
| Event Sourcing          | Store state changes as a sequence of events.                                                 |
| Domain Events           | Represent important business occurrences.                                                    |
| Exactly-Once Processing | A guarantee requiring careful treatment of duplicates, retries, and side effects.            |
| At-Least-Once Delivery  | Messages may be delivered more than once.                                                    |
| At-Most-Once Delivery   | Messages are delivered zero or one time.                                                     |
| Idempotency Key         | Prevent duplicate processing of retried requests.                                            |
| Distributed Locking     | Coordinate access to shared resources across instances.                                      |
| Eventual Recovery       | Restore service after transient failure.                                                     |
| Fault Tolerance         | Continue operating despite component failures.                                               |
| Resilience              | Recover from failures and maintain acceptable service.                                       |
| SLI                     | Service Level Indicator; a measured reliability metric.                                      |
| SLO                     | Service Level Objective; a target for an SLI.                                                |
| SLA                     | Service Level Agreement; a formal service commitment.                                        |
| RTO                     | Recovery Time Objective.                                                                     |
| RPO                     | Recovery Point Objective.                                                                    |

# **15. Special code review principles for Python**

Since Python is commonly used in backend and AI engineering, these are particularly important.

| Concept / Rule             | Explanation                                                   |
| -------------------------- | ------------------------------------------------------------- |
| PEP 8                      | Python style guidelines.                                      |
| PEP 20                     | The Zen of Python.                                            |
| Type Hints                 | Declare expected parameter and return types.                  |
| Dataclasses                | Reduce boilerplate for data-holding classes.                  |
| Pydantic Validation        | Validate and parse structured data.                           |
| Context Managers           | Manage resources using `with`.                                |
| Generator                  | Produce values lazily using `yield`.                          |
| Iterator Protocol          | Implement `__iter__()` and `__next__()`.                      |
| Decorators                 | Wrap or extend function behavior.                             |
| `*args` / `**kwargs`       | Accept variable positional and keyword arguments.             |
| Mutable Default Argument   | Avoid shared mutable defaults.                                |
| EAFP                       | Easier to Ask Forgiveness than Permission.                    |
| LBYL                       | Look Before You Leap.                                         |
| GIL                        | Global Interpreter Lock in conventional CPython builds.       |
| Shallow Copy               | Copy outer container but retain references to nested objects. |
| Deep Copy                  | Recursively copy nested objects.                              |
| `is` vs `==`               | Identity comparison versus equality comparison.               |
| `__repr__`                 | Developer-oriented object representation.                     |
| `__str__`                  | Human-readable object representation.                         |
| Abstract Base Class        | Define contracts for subclasses.                              |
| Protocol                   | Define structural interfaces using typing.                    |
| Resource Cleanup           | Use context managers and `finally` where appropriate.         |
| Avoid Bare Except          | Catch appropriate exception types.                            |
| Avoid Global Mutable State | Reduce hidden shared-state dependencies.                      |
| Avoid Circular Imports     | Keep module dependencies well structured.                     |

# **16. Practical code review checklist**

This is the checklist you can apply whenever reviewing a pull request.

**PR review checklist**

0/30 checked

**1. Correctness and logic**

Does the implementation satisfy the requirements?

Are edge cases and boundary conditions handled?

Are null, empty, and invalid inputs handled?

Are business rules and invariants preserved?

Are exceptions handled correctly?

**2. Code quality**

Are naming and functions meaningful?

Does each class follow SRP?

Is there unnecessary duplication?

Is the code unnecessarily complex?

Are abstractions and design patterns justified?

**3. Architecture**

Are responsibilities separated correctly?

Are dependencies injected appropriately?

Is coupling minimized?

Are interfaces and contracts consistent?

Does the implementation follow existing architecture?

**4. Database and performance**

Are database queries optimized?

Is there an N+1 query problem?

Are transactions and rollbacks correct?

Are resources released properly?

Is caching or pagination required?

**5. Security**

Are inputs validated?

Are authentication and authorization correct?

Are secrets protected?

Are sensitive fields excluded from logs and responses?

Are SQL injection and other security risks prevented?

**6. Testing and deployment**

Are unit and integration tests included?

Are failure scenarios tested?

Are regression tests included?

Do linting, typing, and CI checks pass?

Are migrations and backward compatibility considered?

0% completed

Reset checklist

# **17. Most important concepts to remember for interviews**

If you are preparing for senior Python/backend developer or system design interviews, prioritize these names:

| Area                | Must-know concepts                                                   |
| ------------------- | -------------------------------------------------------------------- |
| Clean Code          | DRY, KISS, YAGNI, Boy Scout Rule                                     |
| OOP                 | SOLID, Encapsulation, Abstraction, Polymorphism                      |
| Architecture        | SoC, DI, IoC, Clean Architecture, Hexagonal Architecture             |
| Design Patterns     | Strategy, Factory, Adapter, Repository, Observer                     |
| Code Smells         | God Class, Feature Envy, Shotgun Surgery, Primitive Obsession        |
| Refactoring         | Extract Method, Guard Clauses, Replace Conditional with Polymorphism |
| Correctness         | Preconditions, Postconditions, Invariants, Idempotency               |
| Testing             | Unit, Integration, E2E, Mocking, Mutation Testing                    |
| Security            | OWASP, Least Privilege, RBAC, ABAC, SAST                             |
| Performance         | Big O, N+1, Caching, Indexing, Connection Pooling                    |
| Distributed Systems | CAP, ACID, Saga, CQRS, Outbox, Event Sourcing                        |
| Reliability         | Circuit Breaker, Retry, Backoff, Bulkhead, Graceful Degradation      |
| Review Process      | LGTM, Blocking Comments, Nitpick, Code Owners, Quality Gates         |

**One important distinction:** A code review is not a competition to apply as many design patterns and principles as possible. The goal is to identify meaningful risks and improve the code without introducing unnecessary complexity.

For practical learning, I can next create a **real-world code review exercise** with intentionally flawed Python/FastAPI code, followed by senior-engineer-style review comments and corrected implementation.

🧑‍💻Practice code reviews dailyTap to set up
