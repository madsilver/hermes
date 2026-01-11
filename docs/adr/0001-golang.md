# ADR-0001: Choice of golang as the language for the distributed chat system

## Status
Proposed

## Context
The project aims to build a distributed chat system composed of multiple server nodes, using a custom communication 
protocol. The system must handle multiple concurrent clients, support real-time message delivery, and enable 
server-to-server communication.

Key characteristics of the system include:
- Network-intensive communication (TCP/WebSocket)
- High levels of concurrency
- Low-latency requirements
- Explicit definition and evolution of a custom protocol
- Fault tolerance and horizontal scalability

Two primary implementation languages were evaluated: java and golang.

---

## Decision
The project will use golang as the primary programming language for implementing the distributed chat system.

---

## Rationale

### 1. Concurrency model well-suited for distributed systems
Go provides goroutines and channels as first-class concurrency primitives, enabling:
- Lightweight concurrent execution
- Simple and expressive handling of many simultaneous connections
- Safe communication between concurrent tasks without complex locking mechanisms

This model aligns naturally with a distributed chat architecture, where each client connection, message exchange, 
and inter-server communication can be handled concurrently.

---

### 2. Simplicity in network protocol implementation
Go’s standard library offers strong native support for networking (`net`, `net/http`), allowing:
- Direct implementation of TCP servers
- Explicit control over connections and buffers
- Straightforward message serialization and parsing

This simplicity is especially valuable when designing and evolving a custom protocol, making debugging, testing, and 
versioning easier.

---

### 3. Low latency and predictable runtime behavior
Go’s runtime is optimized for long-running, I/O-bound services, providing:
- A low-latency garbage collector
- Predictable performance under load
- Efficient handling of large numbers of open connections

These characteristics are critical for real-time chat applications.

---

### 4. Operational simplicity and deployment
Go produces single, self-contained binaries, which:
- Simplify deployment across distributed environments
- Reduce external runtime dependencies
- Work well in containerized and bare-metal setups

This supports horizontal scaling and simplifies the operation of multiple independent chat nodes.

---

### 5. Alignment with project goals
The primary goals of the project include:
- Gaining hands-on experience with distributed systems
- Implementing a custom communication protocol
- Maintaining architectural clarity and transparency

Go supports these goals through:
- A small and simple language specification
- Minimal hidden abstractions
- Readable and maintainable code

---

## Alternatives Considered

### Java
Java was considered due to its:
- Mature ecosystem
- Strong performance characteristics
- Proven use in large-scale production systems

However, for this project, Java presented several drawbacks:
- Higher verbosity
- Increased complexity for asynchronous I/O
- Dependence on frameworks (e.g., Netty) to reach comparable simplicity
- Slower prototyping when designing a custom protocol

While Java is well-suited for enterprise-scale systems, it was deemed more complex than necessary for the current 
project scope and goals.

---

## Consequences

### Positive
- Simpler and more readable codebase
- Clearer implementation of distributed communication
- Faster development and iteration
- Easier horizontal scalability

### Negative
- Smaller ecosystem compared to Java
- Fewer enterprise-grade libraries out of the box
- Greater responsibility on the team to design testing and observability carefully

---

## Final Decision
Given the system requirements and project objectives, Go (Golang) provides the best balance of simplicity, concurrency, 
performance, and architectural clarity. It is therefore the chosen language for implementing the distributed chat system 
with a custom protocol.
