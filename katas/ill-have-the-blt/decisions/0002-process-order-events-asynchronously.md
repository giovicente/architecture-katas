---
status: accepted
date: 2026-11-22
decision-makers: ["Kata solution author"]
---

# Process Order Events Asynchronously

## Context and Problem Statement

Order creation, restaurant preparation, driver coordination, and external payment outcomes occur at different times. A dedicated intermediary ingestion service was considered but adds an extra hop when event producers already publish usable messages.

## Decision Drivers

* Keep request paths responsive
* Decouple producers and asynchronous workflows
* Centralize valid order-state transitions without unnecessary services

## Considered Options

* Orders API plus event-driven Orders Processor
* Synchronous orchestration in Orders API
* Dedicated event ingestion service before processor

## Decision Outcome

Chosen option: **"Orders API plus event-driven Orders Processor"**, because it best satisfies the decision drivers for this Kata without adding complexity that the available requirements do not justify.

### Consequences

* Orders API handles client-facing HTTP; Orders Processor consumes relevant order, payment, and delivery events.
* Both belong to the same Ordering service boundary and may share its PostgreSQL database and domain rules.
* Order Processor can also publish follow-up commands through an outbox, as in ADR 0003.

### Confirmation

* Demonstrate event-driven flow and delayed/duplicate event handling
* Check that ingestion is not deployed without a distinct need

## Pros and Cons of the Options

### Orders API plus event-driven Orders Processor

* Good, because clear separation of synchronous commands and asynchronous reactions
* Good, because processor can scale as a consumer group
* Bad, because requires handling duplicates, ordering, and eventual consistency

### Synchronous orchestration in Orders API

* Good, because simpler debugging for small flows
* Bad, because couples client response time to downstream systems

### Dedicated event ingestion service before processor

* Good, because useful for heterogeneous external payload normalization
* Bad, because unnecessary extra deployment/hop for already normalized internal events

## More Information

* Scope: architectural proposal for the **I’ll Have the BLT** Kata; no deployed implementation is claimed.
* Related: [ADR 0001](0001-use-postgresql-for-order-persistence.md), [ADR 0003](0003-publish-messages-with-transactional-outbox.md).
