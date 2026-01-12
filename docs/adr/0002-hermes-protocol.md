# ADR-0002: Definition of the Hermes Protocol (HP)

## Status
Proposed

## Context
The project consists of a distributed chat system composed of multiple cooperating servers that provide real-time communication between users. The system must support:

- Client–server and server–server communication
- User distribution across multiple nodes
- Message forwarding between servers
- Incremental evolution without breaking compatibility
- Ease of debugging and learning

Given these requirements, there was a need to define a custom communication protocol that provides full control over message formats, communication flows, and distribution decisions.

---

## Decision
The Hermes Protocol (HP) was defined and adopted as the official communication protocol for the system.

HP is a protocol that is:
- TCP-based
- Message-oriented
- Encoded in JSON (UTF-8)
- Explicitly versioned
- Applicable to both client–server and server–server communication

---

## Rationale

### 1. Full control over communication
Defining a custom protocol allows the system to:
- Explicitly define message types and flows
- Control login, delivery, forwarding, and acknowledgment semantics
- Implement distribution and routing rules without relying on generic protocols

This level of control is essential for a distributed system where inter-node behavior must be deterministic and transparent.

---

### 2. Simplicity and readability
HP uses line-delimited JSON to:
- Enable human-readable messages
- Simplify logging and debugging
- Allow manual testing using simple tools (e.g., netcat, telnet)

This choice lowers the cognitive load during development and facilitates troubleshooting.

---

### 3. Distribution as a first-class concern
HP explicitly defines messages for:
- Server discovery and handshakes
- User location propagation
- Inter-server message forwarding
- Health monitoring via heartbeats

This makes distribution a core property of the protocol rather than an implementation detail.

---

### 4. Explicit versioning
Every HP message includes a mandatory protocol version field, which:
- Enables gradual protocol evolution
- Prevents silent incompatibilities
- Supports coexistence of different client and server versions

This decision is critical for long-term maintainability.

---

### 5. Alignment with project goals
The primary goals of the project are:
- Practical learning of distributed systems
- Architectural clarity
- Explicit implementation of network protocols

HP supports these goals by:
- Avoiding excessive abstraction
- Making communication flows explicit
- Encouraging a deep understanding of data exchange between nodes

---

## Alternatives Considered

### Existing protocols (HTTP, WebSocket, gRPC)
Well-established protocols were considered but rejected in this context because they:
- Introduce additional abstraction layers
- Hide important distributed system mechanics
- Reduce direct control over message flows

Although suitable for production systems, they do not align well with the learning-oriented goals of the project.

---

### Binary protocols (e.g., Protobuf)
Binary encodings provide better efficiency but were not selected initially due to:
- Increased debugging complexity
- Tooling overhead
- Reduced transparency during early development

Binary encoding remains a possible future evolution of HP.

---

## Consequences

### Positive
- Clear and predictable communication
- Easier debugging and testing
- Explicit support for distributed architectures
- Protocol extensibility and versioning
- Independence from external frameworks

### Negative
- Responsibility for maintaining protocol correctness
- Higher message overhead due to JSON encoding
- Need for discipline to preserve backward compatibility

---

## Final Decision
The Hermes Protocol (HP) was adopted as the system’s communication protocol because it offers the best balance between simplicity, control, architectural clarity, and distributed-system support, fully meeting both technical and educational objectives of the project.
