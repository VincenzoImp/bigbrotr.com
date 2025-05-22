---
title: System Architecture
description: An in-depth look into Bigbrotr’s modular infrastructure and its core components
---

Bigbrotr is a modular and scalable infrastructure designed to efficiently and resiliently archive and monitor the entire Nostr network. Each component runs in a dedicated Docker container, communicating and coordinating through a centralized PostgreSQL database.

This architecture ensures transparency, reliability, and ease of extension — essential to support a decentralized ecosystem that is constantly evolving.

---

## 🔧 Main Components

### 1. `bigbrotr-database`
The core of the infrastructure: a PostgreSQL instance optimized to handle complex data and large volumes.
- **events**: complete archive of all public Nostr events, indexed for fast querying.
- **events_relays**: maps relationships between events and the relays where they were seen, enabling redundancy and distribution analysis.
- **relays**: list and classification of known relays, tagged by clearnet or Tor network.
- **relay_metadata**: statistical data collected through monitoring, including response times, accessibility, and up-to-date NIP-11 information.

### 2. `torproxy`
A proxy service enabling access to `.onion` Tor relays, ensuring archival and monitoring of hidden nodes and thus a full and inclusive view of the ecosystem.

### 3. `bigbrotr-finder`
Module responsible for dynamic and continuous discovery of new relays:
- Extracts new `wss://` URLs from events and external sources such as Nostr.Watch
- Automatically updates and expands the relay index
- Supports network growth and resilience by identifying new nodes and preventing centralization

### 4. `bigbrotr-monitor`
An active monitoring system that checks the health of each relay daily:
- Performs ping and access tests (open/read/write) while recording round-trip times (RTT)
- Gathers metadata via NIP-11 to assess features, software versions, privacy policies, and NIP-66 compatibility
- Provides essential data to evaluate relay reliability and performance, supporting operators and users

### 5. `bigbrotr-synchronizer`
The synchronization component that guarantees a complete and up-to-date archive:
- Downloads and indexes all new events from every active relay
- Uses a “divide and conquer” algorithm to efficiently handle incomplete or delayed data
- Balances load across relays to optimize performance and maintain chronological event order
- Ensures the integrity and completeness of the event database, essential for research, analysis, and advanced services

---

## 📈 Database Schema

```yaml
events:
  - id (PK)
  - pubkey
  - kind
  - created_at
  - content
  - tags (JSONB)
  - sig

events_relays:
  - event_id (FK)
  - relay_url (FK)
  - seen_at

relays:
  - url (PK)
  - network (clearnet/tor)
  - inserted_at

relay_metadata:
  - relay_url (FK)
  - generated_at
  - connection_success, nip11_success
  - openable, readable, writable
  - rtt_open, rtt_read, rtt_write
  - name, description, banner, icon
  - pubkey, contact, software, version
  - supported_nips (JSONB)
  - privacy_policy, terms_of_service
  - limitations, extra_fields (JSONB)
```

## 🧠 Design Principles

- **Resilience**: data recovery and incremental sync mechanisms to guarantee completeness even during outages or offline relays.

- **Scalability**: modular containerized architecture supporting horizontal scaling and easy maintenance.

- **Extensibility**: designed to integrate public APIs, interactive dashboards, bots, and alerting tools.

- **Research-Oriented**: data normalized and structured to enable advanced analysis like social graph studies, spam detection, and protocol research.

This architecture enables Bigbrotr to be a robust, transparent, and collaborative solution supporting decentralization and open research within the Nostr ecosystem.
