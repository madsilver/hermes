# Hermes

Hermes is a distributed P2P chat system, designed to explore modern networking concepts, binary protocols, and 
distributed systems.

---

## Architectural Overview

* Peers connect directly to each other.
* A bootstrap server is used only for:

    * peer discovery
    * NAT traversal assistance (hole punching)
* The bootstrap server never routes messages.

Once peers are discovered, all communication happens peer‑to‑peer.

---

## Transport Layer

Hermes uses QUIC over UDP as its transport protocol.

---

## License

Apache License 2.0

---

## Project Name

Hermes — messenger of the gods, symbolizing fast and reliable communication.
