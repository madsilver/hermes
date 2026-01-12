# ADR-0003: Hermes Protocol Message Format

## Status
Accepted

## Context
Hermes is a distributed chat system built around a **custom application-layer protocol**, the **Hermes Protocol (HP)**, running directly over raw TCP.  
Because HP is used for both **client–server** and **server–server** communication, the **message format** is a critical architectural decision.

The message format must support:
- Real-time, bidirectional communication
- Clear semantics for many message types
- Easy debugging and inspection
- Incremental evolution and backward compatibility
- Reliable message identification in a distributed environment

A consistent and well-defined message structure is required to keep the protocol simple, extensible, and understandable.

---

## Decision
All Hermes Protocol messages will use a **self-describing, JSON-based message format**, with a **fixed top-level structure** and a **type-specific payload**.

Messages are:
- Encoded as **UTF-8 JSON**
- Framed using **line-delimited messages (`\n`)**
- Identified by a **globally unique message ID**
- Explicitly versioned

---

## Message Structure

Every HP message MUST conform to the following top-level structure:

```json
{
  "version": "1.0",
  "type": "MESSAGE_TYPE",
  "id": "uuid",
  "timestamp": 1700000000,
  "source": "client|server",
  "payload": {}
}
```

### Field Definitions

| Field       | Description |
|------------|-------------|
| `version`   | Protocol version of the message |
| `type`      | Message type identifier |
| `id`        | Globally unique message identifier |
| `timestamp` | Unix epoch time (seconds) |
| `source`    | Origin of the message (`client` or `server`) |
| `payload`   | Type-specific message content |

The `payload` field is intentionally unstructured at the top level and is defined by the semantics of each message type.

---

## Rationale

### 1. Explicit and self-describing messages
Using a fixed top-level structure ensures that every message:
- Can be validated consistently
- Carries enough metadata for routing, logging, and debugging
- Is understandable without external context

This is especially important in a distributed system where messages may cross multiple nodes.

---

### 2. JSON for readability and debuggability
JSON was chosen because it:
- Is human-readable
- Is widely supported
- Simplifies logging and inspection
- Allows manual testing with basic TCP tools

While JSON introduces some overhead, the benefits in clarity and learning outweigh the cost for the current scope of Hermes.

---

### 3. Clear separation of metadata and payload
Separating protocol metadata from message content:
- Keeps routing and protocol logic independent of application data
- Allows new message types to be added without changing the core structure
- Reduces coupling between protocol evolution and application features

---

### 4. Global message identification
The `id` field enables:
- Message acknowledgment (ACK)
- Deduplication
- Retry and idempotent processing
- Tracing messages across servers

This is essential for correctness in a distributed environment.

---

### 5. Explicit versioning
Including the protocol version in every message:
- Prevents silent incompatibilities
- Enables gradual rollout of new protocol versions
- Makes protocol evolution explicit and auditable

---

## Alternatives Considered

### Binary message formats (e.g. Protobuf, FlatBuffers)
Binary formats were considered due to:
- Lower bandwidth usage
- Faster serialization/deserialization

They were rejected at this stage because:
- They reduce human readability
- They complicate debugging
- They introduce additional tooling and schemas

Binary encoding remains a possible future evolution of HP.

---

### Ad-hoc or loosely structured JSON
Allowing free-form or partially structured messages was rejected because it:
- Increases ambiguity
- Makes validation difficult
- Leads to protocol drift over time

A strict top-level structure enforces consistency and discipline.

---

## Consequences

### Positive
- Clear and consistent message handling
- Easier debugging and observability
- Simple protocol evolution
- Strong alignment with distributed-systems learning goals
- Uniform handling for client and server messages

### Negative
- Higher message size compared to binary formats
- Additional parsing overhead
- Requires discipline to maintain backward compatibility

---

## Final Decision
The Hermes Protocol adopts a **strict, JSON-based, self-describing message format** with a fixed top-level structure and type-specific payloads.

This format provides the best balance between **clarity, extensibility, correctness, and educational value**, while remaining suitable for a distributed, real-time messaging system.
