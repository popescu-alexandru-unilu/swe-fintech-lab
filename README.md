# SWE Forge

**Software Engineering — From First Principles to Production-Oriented Financial Systems**

> A Java-first, evidence-driven engineering laboratory. Build one coherent fintech platform while systematically learning and demonstrating computer science, programming paradigms, design principles, architectures, frameworks, infrastructure, testing, security, and operations.

**Status:** Roadmap / work in progress  
**Core language:** Java (choose a supported LTS release; Java 21+ recommended)  
**Primary application:** simulated payments, accounts, double-entry ledger, transfers, settlement and reporting  
**Audience:** software engineering interviews, especially backend, platform and financial technology roles

> [!IMPORTANT]
> This repository is an **engineering portfolio**, not a claim that every listed technology belongs in one production deployment. Alternatives are implemented in isolated modules or branches, compared with evidence, and adopted only when justified. All payments and funds are simulated; no real customer data or live payment rails.

## North star

Build a working financial platform in which a transfer can be requested, authorised, posted to a double-entry ledger, settled, audited, observed, and recovered from failures. Demonstrate *why* each engineering choice is correct, not merely that it compiles.

### Definition of done — applies to **every checkbox**

Mark an item complete only when you can:

1. **Explain** the underlying concept and when *not* to use it.
2. **Implement or configure** it in this repository (or a clearly labelled isolated experiment when inappropriate for the main product).
3. **Verify** it using appropriate unit, integration, property, contract, load, security or failure tests.
4. **Document evidence**: code path, test command, result and trade-off in [`docs/evidence/`](docs/evidence/). Benchmark when the item makes a performance claim.
5. **Defend** the decision in an interview without relying on framework magic.

For purely conceptual topics (for example CAP or consensus), an executable simulation or worked, testable design exercise can count as implementation. For tools, include reproducible commands and configuration. A checked item without evidence is **not complete**.

**Evidence template:** `docs/evidence/<topic>.md` with *Problem → Concept → Implementation → Tests → Results → Trade-offs → Interview explanation*.

## Platform target

```text
Client (optional React/TypeScript UI)
                 |
           API gateway / edge
                 |
      +----------+-----------+
      |                      |
  REST / MVC API       Reactive API lab
      |                      |
      +----------+-----------+
                 |
      Application services / policies
                 |
     Accounts — Payments — Ledger — Risk
                 |
           PostgreSQL
                 |
       Transactional outbox
                 |
             Kafka
                 |
    Settlement / Notifications / Reporting
                 |
    OpenTelemetry → metrics, traces, logs
```

Start as a **modular monolith**. Split components into services only when a measured reason exists. Keep business rules independent of delivery frameworks.

### Critical business invariants

- Every posted journal entry balances: **sum(debits) = sum(credits)** for the same currency.
- A committed transfer changes balances **exactly once**, including after client retries.
- Funds cannot silently disappear or be created by retries, concurrency or partial failures.
- Monetary calculations use exact representations (`BigDecimal` with explicit scale/rounding, or integer minor units); never binary floating point for ledger amounts.
- Every money movement has immutable, attributable, reconstructable audit evidence.
- A failed operation cannot leave a partially committed ledger state.
- API and event contracts are versioned; errors and transient failures have documented semantics.

## Delivery milestones

| Milestone | Working outcome | Primary evidence |
|---|---|---|
| **M0 — Engineering baseline** | Reproducible build, quality gates, CI, ADRs | Clean build from fresh clone |
| **M1 — Correct domain** | Accounts, transfers, double-entry journal, invariants | Unit/property tests |
| **M2 — Real backend** | REST API, persistence, migrations, authentication | Integration/contract tests |
| **M3 — Safe under load** | Concurrent posting, idempotency, locking strategies | Race tests + benchmark |
| **M4 — Asynchronous system** | Outbox, Kafka, consumers, retry and DLQ | Failure/recovery tests |
| **M5 — Paradigm comparison** | MVC vs virtual threads vs reactive WebFlux | Comparable p95/p99 and resource data |
| **M6 — Operable platform** | Docker deployment, telemetry, CI/CD, threat model | Dashboards, runbooks, drills |
| **M7 — Interview-quality case study** | Architecture, reproducibility, comparative results | Demo + design review + write-up |

## Roadmap & individual checklists

Work roughly in order. **Core** items support the platform directly; **extension** items explore trade-offs without bloating the main application.

### 0. Engineering workflow and repository foundations

- [ ] **Git fundamentals** — commits, branches, merges, rebases, resets, tags and history investigation.
- [ ] **GitHub workflow** — issues, pull requests, reviews, protected branches and release notes.
- [ ] **Command line** — shells, pipes, redirects, environment variables, process inspection.
- [ ] **Linux fundamentals** — permissions, filesystems, signals, services, ports and logs.
- [ ] **Build system: Maven** — lifecycle, modules, dependencies, BOMs and plugins.
- [ ] **Build system: Gradle (comparison)** — recreate a small module and document differences.
- [ ] **Java toolchains** — reproducible supported JDK selection and version pinning.
- [ ] **Dependency management** — transitive dependencies, conflicts, upgrades and vulnerability checks.
- [ ] **Repository conventions** — clear modules, package names, `.gitignore`, editor settings.
- [ ] **Formatting** — formatter and formatting check in CI.
- [ ] **Static checks** — Checkstyle, SpotBugs, PMD or equivalent with actionable configuration.
- [ ] **SonarQube/SonarCloud** — inspect and resolve meaningful findings.
- [ ] **Documentation** — getting started, architecture, examples and troubleshooting.
- [ ] **Architecture decision records (ADRs)** — at least three decisions with rejected alternatives.
- [ ] **Conventional development workflow** — small changes, code review checklist and change log.
- [ ] **Reproducible build** — fresh clone → one documented command → successful tests.

### 1. Computer science, algorithms and operating systems

- [ ] **Big-O analysis** — reason about runtime and space; verify with measurements.
- [ ] **Arrays and contiguous memory** — access patterns and resizing costs.
- [ ] **Linked lists** — implementation and practical trade-offs.
- [ ] **Stacks and queues** — implement and use in application workflows.
- [ ] **Hash tables** — collisions, load factor, hashing and equality.
- [ ] **Trees / binary search trees** — traversal, ordering and complexity.
- [ ] **Balanced trees** — understand balancing and when ordered maps help.
- [ ] **Heaps / priority queues** — priority-based scheduling example.
- [ ] **Graphs** — adjacency lists/matrices and financial relationship modelling.
- [ ] **BFS and DFS** — correctness and complexity tests.
- [ ] **Sorting algorithms** — at least merge, quick and heap sort; compare trade-offs.
- [ ] **Binary search** — prove boundaries and test off-by-one errors.
- [ ] **Recursion and iteration** — stack usage and termination.
- [ ] **Dynamic programming** — one nontrivial problem with state design.
- [ ] **Greedy algorithms** — one example and counterexample.
- [ ] **Data encoding** — bytes, Unicode/UTF-8, numeric representations and precision.
- [ ] **Processes vs threads** — isolation, scheduling, context switching.
- [ ] **Virtual memory** — address spaces, pages, translation and page faults.
- [ ] **Memory hierarchy** — registers, caches, RAM, disk; locality.
- [ ] **Caching and cache invalidation** — measure hit rates and stale-data behaviour.
- [ ] **Filesystems and I/O** — buffering, fsync, durability and file descriptors.
- [ ] **Blocking vs non-blocking I/O** — experiment with concurrency and resource use.
- [ ] **Sockets** — write a minimal TCP client/server experiment.
- [ ] **DNS** — name resolution, caching and failure modes.
- [ ] **TCP** — connection lifecycle, flow control, retransmission, ordering.
- [ ] **UDP** — datagram trade-offs and appropriate scenarios.
- [ ] **TLS** — handshakes, certificates and authenticated encryption basics.
- [ ] **HTTP semantics** — methods, status codes, headers, caching and connection reuse.

### 2. Java language and JVM mastery

- [ ] **Types, primitives and references** — identity, value, nullability and conversions.
- [ ] **Control flow and methods** — contracts, overloads and scope.
- [ ] **Classes and constructors** — initialization and invariants.
- [ ] **Encapsulation and access control** — protect valid domain state.
- [ ] **Inheritance** — appropriate substitution and misuse examples.
- [ ] **Polymorphism** — interface and runtime dispatch behaviour.
- [ ] **Interfaces and abstract classes** — selection criteria.
- [ ] **Records** — immutable value carriers and their limitations.
- [ ] **Enums** — safe finite states and behavioural variants.
- [ ] **Sealed types** — model closed state hierarchies.
- [ ] **Generics** — parameterisation, bounds and type erasure.
- [ ] **PECS / wildcards** — covariance and contravariance exercises.
- [ ] **Collections framework** — List, Set, Map, Deque and their implementations.
- [ ] **Equality and hashing** — `equals`, `hashCode`, identity and entity equality.
- [ ] **Ordering** — Comparable, Comparator and consistent comparisons.
- [ ] **Immutability** — mutable vs immutable domain design and defensive copying.
- [ ] **Exceptions** — checked/unchecked, domain failures and exception hygiene.
- [ ] **Try-with-resources** — deterministic resource cleanup.
- [ ] **Functional interfaces** — lambdas and method references.
- [ ] **Streams** — mapping, filtering, reduction, collectors and side-effect risks.
- [ ] **Parallel streams** — benchmark and explain when *not* to use.
- [ ] **Optional** — expressive absence without replacing every reference.
- [ ] **Date/time API** — `Instant`, `LocalDate`, time zones and clock injection.
- [ ] **BigDecimal and monetary precision** — scale, rounding and currencies.
- [ ] **Annotations** — built-in and custom annotations.
- [ ] **Reflection** — use and measure, including design costs.
- [ ] **Java I/O and NIO** — buffers, channels, paths and streaming files.
- [ ] **Serialization** — JSON/schema evolution; explain Java native serialization risks.
- [ ] **Modules (JPMS)** — create boundaries or document why not adopted.
- [ ] **JVM bytecode** — inspect compiled code with `javap`.
- [ ] **JVM execution** — class loading, verification, interpretation and JIT.
- [ ] **Heap and stack** — allocations, call frames and escaping objects.
- [ ] **Garbage collection** — major collectors, pauses and profiling evidence.
- [ ] **JVM diagnostics** — `jcmd`, thread dumps, heap dumps and Java Flight Recorder.
- [ ] **JVM tuning** — measure before changing settings and record results.

### 3. Programming paradigms and foundational design principles

#### Paradigms

- [ ] **Imperative programming** — express transfer workflow as explicit steps.
- [ ] **Procedural programming** — isolate functions and compare modularity.
- [ ] **Object-oriented programming** — behaviour-rich domain objects with invariants.
- [ ] **Functional programming** — pure calculations, higher-order functions and composition.
- [ ] **Declarative programming** — express what is desired (SQL, validation, configuration).
- [ ] **Event-driven programming** — domain events and handlers.
- [ ] **Asynchronous programming** — futures/callbacks, completion and error handling.
- [ ] **Reactive programming** — streams, signals, composition and demand.
- [ ] **Data-oriented design** — data layout and transformation trade-offs.
- [ ] **Composition vs inheritance** — justify both using an implemented example.

#### Principles

- [ ] **SOLID: Single Responsibility Principle (SRP)** — separate independent reasons for change.
- [ ] **SOLID: Open/Closed Principle (OCP)** — extend pricing/risk policy without rewriting callers.
- [ ] **SOLID: Liskov Substitution Principle (LSP)** — verify substitutable implementations via shared tests.
- [ ] **SOLID: Interface Segregation Principle (ISP)** — avoid oversized client contracts.
- [ ] **SOLID: Dependency Inversion Principle (DIP)** — domain depends on abstractions, not adapters.
- [ ] **DRY** — eliminate duplicated business knowledge without premature abstraction.
- [ ] **KISS** — choose the simplest design meeting requirements.
- [ ] **YAGNI** — document deliberately deferred complexity.
- [ ] **Separation of concerns** — distinguish domain, application, delivery and persistence.
- [ ] **High cohesion** — assess module responsibilities.
- [ ] **Loose coupling** — validate changes remain local.
- [ ] **Information hiding** — expose stable APIs, conceal internals.
- [ ] **Law of Demeter** — reduce navigational coupling.
- [ ] **Tell, Don't Ask** — preserve invariants inside the model.
- [ ] **Principle of Least Astonishment** — make APIs predictable.
- [ ] **Fail fast** — reject invalid state at boundaries.
- [ ] **Design by contract** — preconditions, postconditions and invariants.
- [ ] **GRASP: Information Expert** — assign responsibility where knowledge lives.
- [ ] **GRASP: Creator** — justify construction ownership.
- [ ] **GRASP: Controller** — separate input orchestration from domain rules.
- [ ] **GRASP: Low Coupling / High Cohesion** — evaluate a refactoring with concrete examples.
- [ ] **GRASP: Polymorphism / Indirection / Protected Variations** — isolate variable policy.
- [ ] **Clean code and naming** — make intent explicit without unnecessary indirection.
- [ ] **Refactoring discipline** — preserve behaviour through small test-backed changes.

### 4. Design patterns and reusable techniques

#### Creational (GoF)

- [ ] **Factory Method** — polymorphic creation of a payment handler.
- [ ] **Abstract Factory** — families of related adapters in a lab.
- [ ] **Builder** — controlled construction of complex test/domain inputs.
- [ ] **Prototype** — copying semantics and mutable-state hazards.
- [ ] **Singleton** — lifecycle, state and testability drawbacks.

#### Structural (GoF)

- [ ] **Adapter** — map external provider schema to internal ports.
- [ ] **Bridge** — separate independent abstraction dimensions.
- [ ] **Composite** — hierarchical reporting or rules.
- [ ] **Decorator** — add cross-cutting behaviour without modifying core type.
- [ ] **Facade** — simplify multi-step use cases.
- [ ] **Flyweight** — shared immutable state where measured useful.
- [ ] **Proxy** — access control, remoting or lazy access experiment.

#### Behavioural (GoF)

- [ ] **Chain of Responsibility** — validation/risk rule pipeline.
- [ ] **Command** — explicit operation requests and replay considerations.
- [ ] **Interpreter** — small rule language or query evaluator (extension).
- [ ] **Iterator** — controlled traversal and lazy iteration.
- [ ] **Mediator** — reduce direct communication among components.
- [ ] **Memento** — state capture and restoration experiment.
- [ ] **Observer** — notifications and event listeners.
- [ ] **State** — transfer lifecycle with explicit legal transitions.
- [ ] **Strategy** — exchange fee or risk policy selection.
- [ ] **Template Method** — algorithm skeleton and inheritance trade-offs.
- [ ] **Visitor** — operations over a stable type hierarchy.

#### Enterprise techniques

- [ ] **Repository pattern** — persistence abstraction with meaningful boundaries.
- [ ] **Unit of Work** — transaction boundaries and atomic changes.
- [ ] **Dependency Injection / IoC** — composition root and test seams.
- [ ] **Specification pattern** — composable domain rules.
- [ ] **DTO / mapping** — distinguish API schema from domain entities.
- [ ] **Anti-corruption layer** — protect internal model from external conventions.
- [ ] **Strangler Fig pattern** — incremental migration lab (extension).
- [ ] **Code smells** — detect and refactor primitive obsession, feature envy and god objects.

### 5. Domain modelling and fintech correctness

- [ ] **Bounded contexts** — distinguish accounts, payments, ledger, risk and settlement.
- [ ] **Ubiquitous language** — glossary with unambiguous financial terms.
- [ ] **Entities vs value objects** — Account, Transfer, Money, Currency, identifiers.
- [ ] **Aggregate boundaries** — define invariant ownership and transactions.
- [ ] **Domain services** — behaviours not naturally owned by an entity.
- [ ] **Application services** — orchestration vs business logic.
- [ ] **Domain events** — distinguish facts from commands.
- [ ] **Account lifecycle** — open, freeze, close and invalid transitions.
- [ ] **Payment/transfer lifecycle** — pending, authorised, posted, failed, settled.
- [ ] **Double-entry bookkeeping** — balanced debit/credit posting.
- [ ] **Immutable journal** — append-only records; corrections via reversing entries.
- [ ] **Currency safety** — prevent mixing currencies without explicit conversion.
- [ ] **Monetary arithmetic** — exact values, rounding rules and overflow tests.
- [ ] **Available vs posted balance** — model pending holds and posting.
- [ ] **Insufficient-funds policy** — atomic validation under concurrency.
- [ ] **Idempotency keys** — same request/key returns same result, conflicting reuse rejects.
- [ ] **Audit trail** — actor, timestamp, causality, correlation and tamper-evidence strategy.
- [ ] **Reconciliation** — detect discrepancies between ledger and derived balances.
- [ ] **Settlement simulation** — batches, cutoffs and retryable failures.
- [ ] **Fee and pricing rules** — reproducible, versioned policy decisions.
- [ ] **Risk/fraud rules** — simple explainable checks and traceable decisions.
- [ ] **Reporting projections** — rebuild views from the journal.
- [ ] **Regulatory awareness** — research PCI DSS, PSD2/open banking, GDPR and retention; do **not** claim certification or compliance from a demo.
- [ ] **Threat and failure scenarios** — duplicate requests, replay, partial outage, concurrent withdrawal.

### 6. Spring ecosystem, APIs and integration

- [ ] **Spring Core / IoC container** — bean lifecycle, scopes and dependency injection.
- [ ] **Spring Boot configuration** — properties, profiles and external configuration.
- [ ] **Spring MVC** — request lifecycle, controllers and validation.
- [ ] **REST design** — resource modelling, appropriate verbs and status codes.
- [ ] **HTTP idempotency semantics** — distinguish safe methods, idempotent methods and application deduplication.
- [ ] **Request validation** — Bean Validation and domain-level safeguards.
- [ ] **Global exception handling** — consistent RFC 9457-style problem responses.
- [ ] **API pagination** — offset/cursor trade-offs.
- [ ] **Filtering, sorting and search** — stable behaviour and indexing.
- [ ] **API versioning** — backwards compatibility and deprecation policy.
- [ ] **OpenAPI** — published machine-readable contract and examples.
- [ ] **API contract tests** — catch breaking changes automatically.
- [ ] **HTTP clients** — timeouts, pooling, retries and error mapping.
- [ ] **gRPC / Protocol Buffers** — build a small alternative internal API.
- [ ] **GraphQL (extension)** — compare query flexibility, cost and authorization concerns.
- [ ] **WebSockets (extension)** — real-time payment-status updates.
- [ ] **Server-Sent Events (extension)** — one-way event updates; compare to WebSockets.
- [ ] **API gateway / reverse proxy** — routing, headers, TLS termination and limits.
- [ ] **Rate limiting** — per-client fairness and observable rejection.
- [ ] **Caching headers / conditional requests** — ETag and cache correctness where applicable.
- [ ] **Consumer-driven contracts** — e.g. Pact or equivalent for integrations.

### 7. Databases, persistence and data engineering

- [ ] **Relational modelling** — primary keys, foreign keys, constraints and joins.
- [ ] **Normalization** — 1NF–3NF and deliberate denormalization decisions.
- [ ] **PostgreSQL** — schema, roles, connections and operations.
- [ ] **SQL DDL/DML** — schemas, inserts, updates, deletes and upserts.
- [ ] **Joins and aggregations** — correct financial reporting queries.
- [ ] **Indexes** — B-tree, compound indexes and access patterns.
- [ ] **Query plans** — `EXPLAIN (ANALYZE, BUFFERS)` and before/after measurement.
- [ ] **ACID transactions** — atomic ledger posting.
- [ ] **Isolation levels** — read committed, repeatable read, serializable and anomalies.
- [ ] **Optimistic locking** — version conflict and retry behaviour.
- [ ] **Pessimistic locking** — row-level protection and contention measurement.
- [ ] **Deadlocks** — reproduce, diagnose and avoid through ordering.
- [ ] **Constraints and uniqueness** — enforce invariants in the database as well as code.
- [ ] **JDBC** — direct SQL and explicit resource/transaction handling.
- [ ] **JPA/Hibernate** — mappings, persistence context and entity states.
- [ ] **N+1 query problem** — reproduce and eliminate with evidence.
- [ ] **Spring Data JPA** — repository conveniences and query trade-offs.
- [ ] **Database migrations** — Flyway or Liquibase, including rollback/recovery strategy.
- [ ] **Connection pooling** — HikariCP configuration and pool exhaustion test.
- [ ] **Redis** — cache, TTL, eviction and consistency trade-offs.
- [ ] **Cache-aside strategy** — invalidation and stampede protection.
- [ ] **Materialized/read models** — derived projections and rebuild strategy.
- [ ] **Backup and restore** — perform an actual recovery drill.
- [ ] **Data retention and deletion** — privacy obligations vs immutable financial records.
- [ ] **NoSQL comparison (extension)** — document whether a document/key-value store fits any workload.
- [ ] **Database partitioning (extension)** — partition a large journal and measure impact.
- [ ] **Read replicas (extension)** — stale reads and read-your-writes expectations.

### 8. Concurrency, asynchronous and reactive programming

#### Java concurrency

- [ ] **Thread creation and lifecycle** — start, join, interrupt and cancellation.
- [ ] **Java Memory Model** — visibility, ordering and happens-before relations.
- [ ] **Race conditions** — reproduce a lost update with a failing test.
- [ ] **`synchronized` and intrinsic locks** — protect shared state.
- [ ] **`volatile`** — visibility guarantees and limitations.
- [ ] **Atomic variables / CAS** — lock-free counter and contention comparison.
- [ ] **Reentrant locks** — timed acquisition and fairness trade-offs.
- [ ] **Concurrent collections** — correct use of `ConcurrentHashMap` and queues.
- [ ] **Executors / thread pools** — queue sizing, saturation and shutdown.
- [ ] **`CompletableFuture`** — composition, exception propagation and cancellation.
- [ ] **Virtual threads** — high-concurrency blocking I/O with resource measurements.
- [ ] **Structured concurrency (extension)** — where supported, compare lifecycle/cancellation semantics and note preview status if relevant.
- [ ] **Deadlock / livelock / starvation** — reproduce and diagnose.
- [ ] **Backpressure at task queues** — bounded queues and overload behaviour.
- [ ] **Parallelism vs concurrency** — CPU-bound and I/O-bound load experiments.

#### Reactive systems

- [ ] **Reactive Streams specification** — Publisher, Subscriber, Subscription, Processor.
- [ ] **Project Reactor `Mono`** — zero/one-result composition.
- [ ] **Project Reactor `Flux`** — multi-element streams and transformations.
- [ ] **Subscription and demand** — inspect when work starts and how demand propagates.
- [ ] **Backpressure** — bounded consumers and overload handling.
- [ ] **Cold vs hot publishers** — demonstrate subscription behaviour.
- [ ] **Schedulers** — distinguish event-loop execution from bounded elastic/CPU work.
- [ ] **Non-blocking I/O** — identify and eliminate accidental blocking on event loops.
- [ ] **Error operators** — fallback, retry and terminal failures without duplicate financial effects.
- [ ] **Cancellation and resource cleanup** — test subscriber cancellation.
- [ ] **Reactive context propagation** — correlation/tracing without thread-local assumptions.
- [ ] **Spring WebFlux** — implement equivalent transaction-query APIs.
- [ ] **WebClient** — non-blocking downstream integration.
- [ ] **R2DBC** — evaluate fully reactive database access and transaction limits.
- [ ] **RxJava (comparison)** — map equivalent concepts to Reactor in a small lab.
- [ ] **Reactive testing** — Reactor `StepVerifier` and virtual time.
- [ ] **Compare MVC vs virtual threads vs WebFlux** — same semantics, equivalent workload, p50/p95/p99, throughput, CPU, memory and developer complexity.

> **Design guardrail:** Reactive code is not automatically faster; WebFlux + blocking JPA can defeat non-blocking goals. Compare whole request paths fairly. The ledger's correctness must remain invariant across all implementations.

### 9. Architecture, DDD and system design

- [ ] **Layered architecture** — boundaries and dependency direction.
- [ ] **Hexagonal (ports and adapters)** — replace persistence/delivery without changing domain.
- [ ] **Clean Architecture** — dependency rule demonstrated with build-time checks.
- [ ] **Onion architecture** — compare conceptual overlap and differences.
- [ ] **Domain-Driven Design (DDD)** — strategic and tactical patterns applied selectively.
- [ ] **Modular monolith** — module APIs and enforced boundaries.
- [ ] **Spring Modulith (optional)** — verify modules and publish application events.
- [ ] **CQRS** — distinguish write model from reporting projections.
- [ ] **Event Sourcing (extension)** — design a separate experiment and compare to journal-backed ledger.
- [ ] **Microservices (extension)** — split only one justified bounded context.
- [ ] **Service discovery (extension)** — deployment and communication implications.
- [ ] **Synchronous vs asynchronous boundaries** — latency/failure trade-offs.
- [ ] **Architecture fitness functions** — ArchUnit tests for module dependencies.
- [ ] **API schema and event evolution** — compatibility policy, migrations and consumers.
- [ ] **Capacity planning** — estimate QPS, storage growth and bottlenecks.
- [ ] **Architecture diagrams** — C4 context, containers, components and critical sequence flows.
- [ ] **Trade-off records** — latency vs throughput; consistency vs availability; simplicity vs flexibility.

### 10. Distributed systems, messaging and fault tolerance

- [ ] **Distributed-system failure modes** — partial failure, latency, partitions and uncertain outcomes.
- [ ] **CAP theorem** — state precise conditions and limitations; avoid blanket database labels.
- [ ] **Consistency models** — strong, eventual and read-your-writes expectations.
- [ ] **Replication** — leader/follower behaviour and replica lag exercise.
- [ ] **Consensus (extension)** — small Raft-style simulation or rigorous study.
- [ ] **Kafka fundamentals** — topics, partitions, offsets and retention.
- [ ] **Kafka producers** — delivery acknowledgements and durable publication.
- [ ] **Kafka consumers** — groups, rebalancing and offset management.
- [ ] **Message ordering** — key by aggregate and demonstrate partition constraints.
- [ ] **At-least-once delivery** — duplicate processing with safe consumer design.
- [ ] **Exactly-once claims** — distinguish broker guarantees from end-to-end financial effects.
- [ ] **Transactional outbox** — commit journal and event intent atomically.
- [ ] **Inbox / deduplication** — idempotent event consumer.
- [ ] **Retry policy** — exponential backoff, jitter and bounded attempts.
- [ ] **Dead-letter queue (DLQ)** — quarantine and documented replay process.
- [ ] **Poison message handling** — isolate permanently invalid messages.
- [ ] **Event schema versioning** — compatibility checks and migration.
- [ ] **Distributed tracing** — follow one transfer across synchronous/asynchronous hops.
- [ ] **Saga pattern** — explicit compensation for a multi-step simulated settlement.
- [ ] **Two-phase commit (study/extension)** — costs, limitations and comparison with sagas.
- [ ] **Circuit breaker** — trip, half-open, recover under injected failures.
- [ ] **Timeouts** — explicit budgets per dependency.
- [ ] **Bulkheads** — resource isolation between workloads.
- [ ] **Load shedding** — predictable behaviour under overload.
- [ ] **Rate limiting** — distributed vs local limits comparison.
- [ ] **Clock and time issues** — monotonic time, skew, ordering and UTC timestamps.
- [ ] **Failure injection** — kill process/network dependency during transfer and recover safely.

### 11. Security and privacy

- [ ] **Threat modelling** — assets, trust boundaries, STRIDE-style scenarios and mitigations.
- [ ] **Authentication vs authorization** — separate identity from permissions.
- [ ] **Spring Security** — filter chain and security configuration.
- [ ] **OAuth 2.0** — roles, flows and token validation.
- [ ] **OpenID Connect** — identity claims and token types.
- [ ] **JWT** — signature, audience, issuer, expiry and key rotation.
- [ ] **RBAC** — least-privilege roles for customers/operators.
- [ ] **ABAC / object-level authorization** — enforce ownership of every account/transfer.
- [ ] **TLS / HTTPS** — terminate securely and avoid insecure defaults.
- [ ] **Secrets management** — externalise credentials and rotate demo secrets.
- [ ] **Password storage (if used)** — Argon2id/bcrypt and safe verification.
- [ ] **Input validation** — reject malformed values and unsafe state transitions.
- [ ] **SQL injection prevention** — parameterized queries and adversarial tests.
- [ ] **XSS / CSRF** — protection appropriate to browser authentication model.
- [ ] **SSRF (if outbound fetches exist)** — destination allowlisting and restrictions.
- [ ] **OWASP API Security Top 10** — threat-to-test mapping.
- [ ] **Dependency/SBOM scanning** — inspect vulnerabilities and false positives.
- [ ] **SAST / DAST** — automated checks and remediation exercises.
- [ ] **Audit logging security** — prevent secrets/PII leaking into logs.
- [ ] **Encryption at rest** — database/storage protection and key-management model.
- [ ] **PII minimization and retention** — collect only necessary synthetic data.
- [ ] **Security incident playbook** — detection, containment and evidence preservation.

### 12. Testing, verification and quality engineering

- [ ] **JUnit 5** — deterministic unit tests with meaningful names.
- [ ] **Test fixtures / builders** — readable, isolated scenarios.
- [ ] **Mockito / test doubles** — isolate external interactions, avoid overspecification.
- [ ] **AssertJ / expressive assertions** — clear invariant failures.
- [ ] **Parameterised tests** — currency, boundary and state-transition matrices.
- [ ] **Property-based testing** — jqwik or equivalent for balance/invariant properties.
- [ ] **Mutation testing** — PIT catches missing assertions.
- [ ] **Code coverage** — JaCoCo as signal, not a quality target by itself.
- [ ] **Integration tests** — real Postgres with Testcontainers.
- [ ] **Messaging integration tests** — Kafka and restart/duplicate scenarios.
- [ ] **HTTP contract tests** — real request/response shape and compatibility.
- [ ] **End-to-end tests** — create → post → settle → reconcile.
- [ ] **Concurrency tests** — simultaneous withdrawal/duplicate request and no lost money.
- [ ] **Fault injection / chaos tests** — failures at commit, publish and consume boundaries.
- [ ] **Security tests** — authorization, injection and replay attempts.
- [ ] **Performance regression tests** — compare against recorded baselines.
- [ ] **Test pyramid / test strategy** — explicit trade-offs for test layers.
- [ ] **Deterministic time** — injectable `Clock` and stable timestamp tests.
- [ ] **Contract-first design** — requirements and invariants traceable to tests.
- [ ] **Formal methods (extension)** — specify a small state machine using TLA+/Alloy or equivalent.
- [ ] **Model-based testing (extension)** — generate action sequences against the transfer lifecycle.
- [ ] **ArchUnit** — codify architectural rules.
- [ ] **CI quality gates** — reject failing tests, static analysis or security checks.

### 13. Performance and JVM engineering

- [ ] **Performance hypothesis** — define the bottleneck before optimising.
- [ ] **Latency distributions** — p50/p95/p99 and long-tail behaviour.
- [ ] **Throughput and saturation** — measured maximum sustainable load.
- [ ] **Load generation** — k6, Gatling or JMeter with repeatable scenarios.
- [ ] **JMH microbenchmarking** — warmup, forks, dead-code pitfalls and interpretation.
- [ ] **JFR / async-profiler** — CPU and allocation profiles.
- [ ] **Heap dump analysis** — retained objects and leaks.
- [ ] **Garbage-collector analysis** — allocation rate, pauses and tuning.
- [ ] **CPU profiling** — identify hot methods rather than guessing.
- [ ] **Lock contention profiling** — analyse blocking and hot locks.
- [ ] **Database bottleneck analysis** — slow queries, indexes and lock waits.
- [ ] **Connection-pool saturation** — load behaviour and capacity limits.
- [ ] **Caching benchmarks** — benefits, invalidation cost and correctness.
- [ ] **False sharing / cache locality (extension)** — isolated demonstration.
- [ ] **Tail-latency mitigation** — bounded queues, timeout budgets and overload controls.
- [ ] **Benchmark reproducibility** — hardware, JDK, workload, seed and configuration recorded.
- [ ] **Performance comparison report** — MVC vs virtual threads vs WebFlux with identical correctness checks.

### 14. DevOps, cloud, networking and delivery

- [ ] **Docker** — multi-stage build, small image, non-root user and health checks.
- [ ] **Docker Compose** — reproducible app + Postgres + Kafka + telemetry stack.
- [ ] **Twelve-factor principles** — configuration, backing services and disposability.
- [ ] **GitHub Actions** — build, unit/integration tests, lint and scan pipeline.
- [ ] **Artifact versioning** — tags, images and immutable releases.
- [ ] **CI/CD environments** — dev/test/stage separation and controlled promotion.
- [ ] **Release strategies** — rolling, blue-green and canary comparison.
- [ ] **Rollbacks and migrations** — compatible schema release plan.
- [ ] **Infrastructure as Code** — Terraform modules for isolated demo infrastructure.
- [ ] **AWS fundamentals** — IAM, VPC, security groups, S3 and logging.
- [ ] **AWS compute** — deploy via ECS/Fargate or equivalent managed service.
- [ ] **Managed Postgres** — RDS concepts, backups and operations (optional deployed demo).
- [ ] **Kubernetes (extension)** — Deployments, Services, ConfigMaps, Secrets and probes.
- [ ] **Helm (extension)** — deploy reproducible Kubernetes charts.
- [ ] **Networking diagnostics** — `curl`, `dig`, `ss`, `tcpdump` and TLS inspection.
- [ ] **Reverse proxy** — Nginx or equivalent routing and header trust.
- [ ] **Environment configuration** — safe defaults, validation and zero secrets in Git.
- [ ] **Cost awareness** — estimate and cap cloud resource costs.
- [ ] **Disaster recovery** — demonstrate restore within a defined recovery goal.
- [ ] **Runbook-driven deployment** — another engineer can deploy and recover the system.

### 15. Observability, SRE and operations

- [ ] **Structured logs** — JSON with identifiers, without secrets.
- [ ] **Correlation IDs** — preserve request identity across boundaries.
- [ ] **Metrics** — counters, rates, gauges and histograms.
- [ ] **Prometheus** — scrape application and JVM metrics.
- [ ] **Grafana** — service health and financial workflow dashboards.
- [ ] **OpenTelemetry** — instrument traces, metrics and context.
- [ ] **Distributed traces** — visualize REST → database → outbox → Kafka → consumer.
- [ ] **Tracing backend** — Jaeger, Tempo or equivalent.
- [ ] **Log search** — Loki/ELK or equivalent for troubleshooting.
- [ ] **SLIs** — define success rate, latency, backlog and reconciliation lag.
- [ ] **SLOs** — justified target values and measurement windows.
- [ ] **Error budgets** — connect reliability risk with release decisions.
- [ ] **Alerting** — actionable symptoms, not noisy internal events.
- [ ] **Health / readiness / liveness** — correct semantics for each probe.
- [ ] **Incident response** — simulated outage, timeline and postmortem.
- [ ] **Operational runbooks** — Kafka lag, deadlocks, failed settlement, DB exhaustion.
- [ ] **Capacity dashboards** — saturation and growth trends.

### 16. Frontend, full-stack and user experience (extension track)

- [ ] **HTML semantics** — meaningful forms and document structure.
- [ ] **CSS layout** — responsive layouts and design system primitives.
- [ ] **TypeScript** — typed API client and domain-safe view models.
- [ ] **React** — account, payment and activity views.
- [ ] **State management** — server state vs local state and cache invalidation.
- [ ] **Form validation** — user feedback paired with authoritative backend checks.
- [ ] **Authentication UX** — session handling, protected routes and logout.
- [ ] **Accessibility** — keyboard support, focus and WCAG-aligned checks.
- [ ] **Frontend testing** — unit/component and Playwright E2E flows.
- [ ] **API integration** — generated client from OpenAPI or equivalent.
- [ ] **Real-time UI** — SSE/WebSockets with reconnection handling.
- [ ] **Browser security** — CSP, safe rendering and token-storage trade-offs.

### 17. Professional SWE practice and interview evidence

- [ ] **Requirements engineering** — user stories, acceptance criteria and explicit nonfunctional requirements.
- [ ] **Technical specification** — describe one major subsystem before implementation.
- [ ] **RFC / design review** — propose, critique and revise a design.
- [ ] **Trade-off analysis** — compare at least two alternatives using defined criteria.
- [ ] **Code review practice** — document defects caught and why they matter.
- [ ] **Refactoring case study** — before/after tests and complexity changes.
- [ ] **Bug investigation** — reproducible issue, root cause and permanent regression test.
- [ ] **Debugging** — debugger, logs, tracing, thread dump and database inspection.
- [ ] **Maintainability** — explain module responsibilities and extension points.
- [ ] **Reliability story** — demonstrate an induced outage and safe recovery.
- [ ] **Correctness story** — show an invariant holding under adversarial input/concurrency.
- [ ] **Performance story** — show a statistically defensible before/after improvement.
- [ ] **Security story** — show a realistic threat and validated mitigation.
- [ ] **Architecture story** — justify why the project starts as a modular monolith.
- [ ] **Production readiness review** — SLOs, monitoring, security, backup and incident plan.
- [ ] **Clean-room onboarding** — someone else follows README and runs all tests.
- [ ] **Demo walkthrough** — scripted 5-minute transfer + failure + observability scenario.
- [ ] **Interview case study** — architecture diagram, decisions, measurable evidence and lessons.
- [ ] **README evidence dashboard** — link completed items to code, tests, ADRs and benchmarks.

## Evidence index (fill as you complete work)

| Topic | Implementation | Test or reproducible command | Evidence / ADR | Status |
|---|---|---|---|---|
| Balanced ledger | `TBD` | `TBD` | `TBD` | Not started |
| Transfer idempotency | `TBD` | `TBD` | `TBD` | Not started |
| Database isolation | `TBD` | `TBD` | `TBD` | Not started |
| Kafka outbox / deduplication | `TBD` | `TBD` | `TBD` | Not started |
| MVC vs virtual threads vs reactive | `TBD` | `TBD` | `TBD` | Not started |
| Security threat model | `TBD` | `TBD` | `TBD` | Not started |
| Recovery / operational drill | `TBD` | `TBD` | `TBD` | Not started |

## Suggested repository layout

```text
swe-forge/
├── README.md
├── pom.xml                     # Maven multi-module root
├── docs/
│   ├── architecture/           # C4, sequences, diagrams
│   ├── adr/                    # Architecture decisions
│   ├── evidence/               # One completion record per topic
│   ├── benchmarks/             # Method, raw data, analysis
│   ├── security/               # Threat model, test results
│   └── runbooks/               # Operational procedures
├── domain/                     # Java-only model and invariants
├── application/                # Use cases and ports
├── adapters/
│   ├── postgres/               # JDBC/JPA implementations
│   ├── kafka/                  # Outbox publisher + consumers
│   └── external/               # Simulated payment integrations
├── api-mvc/                    # Primary HTTP application
├── api-reactive-lab/           # Comparable WebFlux variant
├── experiments/                # Independent concept labs
│   ├── algorithms/
│   ├── patterns/
│   ├── concurrency/
│   ├── networking/
│   └── rxjava/
├── benchmarks/
├── integration-tests/
├── frontend/                   # Optional React/TypeScript
├── infra/
│   ├── docker/
│   ├── terraform/
│   └── kubernetes/              # Optional
└── .github/workflows/
```

This layout is **aspirational**: create directories/modules only when work requires them. In particular, do **not** create microservices, Kafka infrastructure or a frontend at M0.

## First executable slice

Implement this flow before adding additional frameworks:

1. Create `Money`, `AccountId`, `TransferId`, `JournalEntry`, `Posting` as well-defined domain types.
2. Implement `postTransfer(from, to, amount, idempotencyKey)` with strict validation and balanced postings.
3. Write exhaustive boundary, property and concurrent correctness tests.
4. Expose `POST /transfers` and `GET /transfers/{id}` via Spring MVC.
5. Persist atomically in PostgreSQL, add schema migrations and duplicate-request handling.
6. Make the build, integration tests and local demo run from documented commands.

**Gate before M3:** No amount may be lost, created or posted twice when the same transfer is sent concurrently. Record the exact database constraints, isolation strategy, tests and result.

## Interview presentation

> “SWE Forge is my Java-first engineering portfolio. I built a simulated financial transaction platform and used it to test fundamental design principles, concurrency models, data consistency, distributed messaging, security and performance. Every major claim is tied to implementation, reproducible tests, measurements and documented architectural trade-offs.”

**Quality beats breadth:** 25 thoroughly evidenced concepts are more impressive than 300 ticked boxes without proof. The checklist is a long-term map, not a claim that the platform is already complete.

## License and disclaimer

Choose and add a license before external contributions. This is a learning and demonstration platform, **not** certified banking software, financial advice, or a system designed to handle real funds.
