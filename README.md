# Hermes

Hermes is an open-source distributed chat system focused on learning, clarity, and explicit design.  
It implements a custom application-layer protocol — the Hermes Protocol (HP) — running directly over raw TCP, without HTTP in the core messaging path.

The project is designed to be readable, hackable, and educational, while still following sound distributed-systems principles.

---

## What is Hermes?

Hermes is a hands-on exploration of:

- Distributed systems fundamentals
- Custom network protocols
- Concurrency and message routing
- Failure detection and recovery
- Low-overhead, long-lived TCP connections

Rather than relying on existing abstractions (HTTP, REST, gRPC), Hermes builds the communication layer explicitly so 
contributors can see and understand what’s really happening.

---

## Project Goals

Hermes aims to:

- Be a clear reference implementation of a distributed chat system
- Use a custom protocol designed for stateful, event-driven communication
- Support horizontal scaling via multiple cooperating servers
- Keep the system simple, explicit, and debuggable
- Serve as a learning resource for developers interested in distributed systems

---

## Architecture Overview

Hermes consists of three main components:

### Clients
- Connect to a single server over TCP
- Maintain a long-lived connection
- Send and receive messages in real time

### Servers
- Accept many concurrent client connections
- Track which users are connected locally
- Forward messages to other servers when needed
- Communicate with peers using the same protocol as clients

### Cluster
- Servers form a distributed cluster via TCP
- User presence is propagated between nodes
- Messages are routed based on user location
- Heartbeats are used to detect node failures

---

## Hermes Protocol (HP)

The Hermes Protocol (HP) is a custom, message-oriented protocol with the following properties:

- **Transport**: Raw TCP
- **Encoding**: JSON (UTF-8)
- **Framing**: Line-delimited (`\n`)
- **Model**: Stateful, event-driven
- **Versioning**: Explicit version field in every message

HP is used for:
- Client ↔ Server communication
- Server ↔ Server communication

---

## Technology Stack

- **Language**: Go (Golang)
- **Concurrency**: Goroutines and channels
- **Networking**: TCP (`net` package)
- **Serialization**: JSON
- **Build & Deploy**: Single static binaries

All major technical choices are documented using Architecture Decision Records (ADRs).

---

## Name

Hermes is named after the Greek messenger god — fitting for a system built entirely around message delivery.
