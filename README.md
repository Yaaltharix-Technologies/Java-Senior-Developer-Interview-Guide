# Java Senior Developer Interview Guide

Comprehensive Java interview preparation guide with curated questions and concise answers from **base** level to **super pro**.

---

## Level 1: Base (Fundamentals)

### 1) What is Java, and why is it platform-independent?
Java is a high-level, object-oriented language. It is platform-independent because Java code compiles to bytecode, which runs on the JVM available for different operating systems.

### 2) Difference between JDK, JRE, and JVM?
- **JDK**: Development kit (compiler, tools, JRE).
- **JRE**: Runtime environment (JVM + libraries).
- **JVM**: Virtual machine that executes bytecode.

### 3) What are the main OOP principles in Java?
Encapsulation, Inheritance, Polymorphism, and Abstraction.

### 4) What is the difference between `==` and `.equals()`?
`==` compares references (or primitive values), while `.equals()` compares logical/content equality (if overridden properly).

### 5) Why are `String` objects immutable?
For security, thread safety, string pooling efficiency, and reliable hashing.

### 6) What is method overloading vs method overriding?
- **Overloading**: Same method name, different parameter list (compile-time polymorphism).
- **Overriding**: Subclass provides specific implementation of a superclass method (runtime polymorphism).

### 7) What is the purpose of constructors?
Constructors initialize objects when they are created.

### 8) Difference between `ArrayList` and `LinkedList`?
`ArrayList` is better for random access and is usually preferred in practice. `LinkedList` can be efficient for insert/delete only when you already have an iterator at the position; traversal is still linear and it has higher memory/cache overhead.

### 9) What is `static` in Java?
`static` members belong to the class, not to individual objects.

### 10) What is `final` used for?
- `final` variable: constant
- `final` method: cannot be overridden
- `final` class: cannot be inherited

---

## Level 2: Intermediate

### 1) What is the Java Collections Framework?
A unified architecture of interfaces and classes (`List`, `Set`, `Map`, `Queue`) for storing and manipulating groups of objects.

### 2) Difference between `HashMap` and `ConcurrentHashMap`?
`HashMap` is not thread-safe and allows one `null` key plus `null` values. `ConcurrentHashMap` supports safe concurrent access with high throughput and does not allow `null` keys or values.

### 3) What is a `HashSet` internally?
A wrapper around `HashMap` where elements are stored as keys with a dummy value.

### 4) What are checked and unchecked exceptions?
- **Checked**: Must be handled/declared (e.g., `IOException`)
- **Unchecked**: Runtime exceptions (e.g., `NullPointerException`)

### 5) Explain `try-with-resources`.
It automatically closes resources implementing `AutoCloseable`, reducing leaks and boilerplate.

### 6) What is the difference between `Comparable` and `Comparator`?
- `Comparable`: natural ordering inside class (`compareTo`)
- `Comparator`: external/custom ordering (`compare`)

### 7) What is autoboxing and unboxing?
Automatic conversion between primitives and wrapper classes (e.g., `int` ↔ `Integer`).

### 8) How does garbage collection work at a high level?
The JVM identifies unreachable objects and reclaims memory automatically; modern collectors optimize pause time and throughput.

### 9) What is the difference between `throw` and `throws`?
- `throw`: used to actually throw an exception
- `throws`: used in method signature to declare possible exceptions

### 10) What are Java 8 functional interfaces?
Interfaces with exactly one abstract method (e.g., `Runnable`, `Supplier<T>`), often used with lambdas.

---

## Level 3: Advanced

### 1) Explain the Java Memory Model (JMM).
The JMM defines how threads interact through memory and guarantees visibility/order via rules like `happens-before`.

### 2) What does `volatile` guarantee?
Visibility of writes across threads and ordering constraints; it does not provide atomicity for compound operations.

### 3) Difference between `synchronized`, `Lock`, and atomic classes?
- `synchronized`: built-in monitor locking
- `Lock`: explicit lock API with advanced features (`tryLock`, interruptible)
- Atomic classes: lock-free CAS operations for single variables

### 4) What is `happens-before`?
A relationship ensuring that memory writes by one action are visible to another action.

### 5) What are common GC collectors in modern Java?
G1, ZGC, Shenandoah, and Parallel GC are common options in modern Java runtimes; each balances latency and throughput differently.

### 6) What are streams in Java, and when not to use them?
Streams provide declarative data processing. Avoid them in extremely performance-critical hot paths or where imperative code is clearer.

### 7) What are good and bad uses of `Optional`?
Good: return type to model absence clearly. Bad: fields/parameters overuse, serializable DTOs, or performance-sensitive loops.

### 8) Explain `CompletableFuture`.
A powerful async abstraction for non-blocking pipelines, composition, and error handling.

### 9) What is class loading delegation?
Class loaders follow parent-first delegation to avoid duplicate core class loading and improve security consistency.

### 10) What causes memory leaks in Java?
Unreleased references (caches/listeners/thread-locals/static collections), not lack of manual `free`.

---

## Level 4: Expert

### 1) How would you diagnose high CPU usage in a Java service?
Capture thread dumps, correlate with CPU profiles (JFR/async-profiler), identify hot methods, and verify lock contention or tight loops.

### 2) How would you diagnose OutOfMemoryError?
Check error type, heap dump, GC logs, object histograms, and retention paths to find dominant object graphs.

### 3) Explain safe publication of objects.
Publish via final fields, static initialization, volatile references, or proper synchronization to avoid visibility issues.

### 4) What are false sharing and cache line contention?
Performance degradation when independent variables share a cache line and are modified by different threads.

### 5) Why can double-checked locking fail without `volatile`?
Because instruction reordering may expose a partially constructed object reference to other threads.

### 6) How would you design a resilient Java microservice?
Timeouts, retries with backoff, circuit breakers, bulkheads, idempotency, observability, and graceful degradation.

### 7) What is backpressure in reactive systems?
A mechanism to let consumers control producer speed to prevent overload and uncontrolled buffering.

### 8) How do you tune JVM for low-latency systems?
Use low-pause collectors (ZGC/Shenandoah), size heap carefully, reduce allocations, tune thread pools, and profile continuously.

### 9) What are common pitfalls of thread pools?
Unbounded queues, wrong pool sizing, blocking tasks in CPU pools, missing rejection policies, and context leakage.

### 10) How do you secure Java applications end-to-end?
Input validation, output encoding, strong authz/authn, dependency patching, secret management, and least-privilege defaults.

---

## Level 5: Super Pro (Architecture & Deep JVM)

### 1) How does JVM JIT optimization affect production behavior?
Hot code paths get optimized over time; warmup effects, speculative optimizations, and deoptimizations can change latency characteristics.

### 2) What is escape analysis?
The JIT determines object scope; non-escaping objects may be stack-allocated or scalar-replaced, reducing heap pressure.

### 3) Explain safepoints and their impact.
Safepoints are JVM coordination points for GC and VM operations. Excessive safepoints can increase pause time.

### 4) How would you evaluate virtual threads (Project Loom) adoption?
Assess blocking model compatibility, thread-local usage, pinning risks (for example when a virtual thread blocks while holding a monitor or during certain native/foreign calls), observability tooling, and throughput/latency trade-offs when your target runtime supports virtual threads.

### 5) What is the trade-off between monolith and microservices in Java ecosystems?
Monolith: simpler operations and consistency. Microservices: independent scaling/deployment but higher distributed-system complexity.

### 6) How do you design for idempotency in distributed Java systems?
Use idempotency keys, dedup stores, deterministic handlers, and careful side-effect boundaries.

### 7) How do you select between synchronous and asynchronous messaging?
Choose sync for immediate consistency/response needs; async for decoupling, resilience, and burst smoothing.

### 8) How do you approach performance engineering end-to-end?
Define SLOs, benchmark realistically, profile hotspots, optimize bottlenecks, validate under load, and guard with regression tests.

### 9) What is your strategy for zero-downtime Java deployments?
Blue-green/canary rollout, backward-compatible contracts, health checks, graceful shutdown, and rapid rollback.

### 10) How do you make senior-level technical decisions?
Use explicit trade-offs (cost, risk, complexity, operability), involve stakeholders, run experiments, and document decision records.

---

## Quick Usage Strategy

1. Start with **Base** and ensure clear conceptual understanding.
2. Move to **Intermediate** and practice code-level examples.
3. Use **Advanced+** for system design, concurrency, and JVM internals.
4. For interviews, prepare short and deep versions of each answer.
