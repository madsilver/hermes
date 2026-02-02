# RFC 0001 — Hermes Binary Framing

Status: Draft  
Version: 1.0  
Author: Rodrigo Prata  
Last Updated: 2026-02-02

## 1. Abstract

This document defines the binary framing layer of the Hermes protocol.

Hermes is a P2P distributed chat system built on top of QUIC. The framing layer specified here defines how Hermes messages 
are delimited, versioned, categorized, and extended.

## 2. Goals and Non‑Goals

### 2.1 Goals

The Hermes framing layer is designed to:

- Provide a simple and efficient binary format
- Support stream‑oriented transports (QUIC streams)
- Separate control traffic from application events

### 2.2 Non‑Goals

The framing layer does not define:

- Encryption or authentication (handled by QUIC)
- Reliable delivery (handled by QUIC)


## 3. Stream Model

Hermes defines logical roles for QUIC streams.

### 3.1 Defined Streams (v1)

| Stream    | Purpose          |
|-----------|------------------|
| Stream 0  | Control messages |
| Stream 1  | Event messages   |

Additional streams may be defined in future versions.

## 4. Base Frame Format

All Hermes messages, regardless of stream, MUST use the same base frame format.

```
+--------+--------+--------+--------------+
| Magic  | Ver    | Type   | Length       |
| 2 bytes| 1 byte | 1 byte | uvarint      |
+--------+--------+--------+--------------+
| Payload (Length bytes)                  |
+-----------------------------------------+
```

### 4.1 Magic

- Size: 2 bytes
- Value: `0x48 0x4D` (ASCII "HM")

The Magic field identifies Hermes frames and allows receivers to discard invalid or misaligned data.

Receivers must drop frames with an invalid Magic value.

### 4.2 Version

- Size: 1 byte

The Version field identifies the Hermes protocol version.

- Version `0x01` corresponds to Hermes v1

Receivers may reject frames with unsupported versions.

### 4.3 Type

- Size: 1 byte

The Type field identifies the category of the message.

Defined values (v1):

| Value  | Meaning         |
|--------|-----------------|
| `0x01` | Control message |
| `0x02` | Event message   |

Receivers must ignore unknown Type values by skipping the frame payload using the Length field.

### 4.4 Length

- Encoding: unsigned variable‑length integer (uvarint)

Length specifies the size, in bytes, of the Payload field.

This field allows receivers to skip unknown or unsupported message types safely.

## 5. Control Frames (Stream 0)

Control frames use the base frame format with `Type = 0x01`.

Control frames are used for:

- Protocol handshake
- Presence heartbeats
- Error reporting

Control frames must not include persistent event identifiers (event_id).

## 6. Event Frames (Stream 1)

Event frames use the base frame format with `Type = 0x02`.

The payload of an Event frame must begin with the Event Header.

### 6.1 Event Header Format

```
+--------------------+-------------------+------------------+
| event_id           | event_kind        | event_payload... |
| 16 bytes           | 1 byte            | variable         |
+--------------------+-------------------+------------------+
```

### 6.2 event_id

- Size: 16 bytes
- Generation: random

The event_id uniquely identifies a persistent application‑level event.

The event_id is used for:

- Deduplication
- Acknowledgements (ACK)
- Synchronization (sync)

The event_id must be stable across retransmissions and reconnections.

---

### 6.3 event_kind

- Size: 1 byte

The event_kind field identifies the semantic type of the event.

Examples (non‑exhaustive):

- chat message
- acknowledgement (ACK)
- synchronization request/response

Unknown event_kind values must be ignored while preserving framing correctness.

### 6.4 event_payload

The structure of event_payload depends on event_kind.

This RFC does not define specific event payload formats.

## 7. Acknowledgements and Synchronization

ACK and sync messages are modeled as events, not transport signals.

Therefore:

- ACK events have their own event_id
- ACK payloads reference the event_id being acknowledged
- Sync requests reference known or missing event_id values

This design ensures uniform deduplication, replay safety, and extensibility.

## 8. Error Handling

TBD

## 11. Security Considerations

- All Hermes frames rely on QUIC for encryption and integrity
- event_id values must be unpredictable


