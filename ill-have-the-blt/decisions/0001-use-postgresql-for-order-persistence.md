---
status: accepted
date: 2026-11-22
decision-makers: ["Kata solution author"]
---

# Use PostgreSQL for Order Persistence

## Context and Problem Statement

Orders need a reliable current state, auditable transitions, and transactional updates. The exercise targets millions of users, but provides no measured throughput that would justify selecting a distributed NoSQL store by default.

## Decision Drivers

* Preserve consistency across order and transition history
* Support order lookup and operational queries
* Avoid complexity unsupported by capacity estimates

## Considered Options

* PostgreSQL with current-state and append-only history tables
* Document-oriented NoSQL with mutable order documents
* Full event sourcing

## Decision Outcome

Chosen option: **"PostgreSQL with current-state and append-only history tables"**, because it best satisfies the decision drivers for this Kata without adding complexity that the available requirements do not justify.

### Consequences

* Store one current-state row per order and append a history row for each accepted transition in the same transaction.
* Use conditional updates or versioning to prevent invalid concurrent transitions.
* Event sourcing is not adopted; the status-history table is an audit trail, not the source of truth.

### Confirmation

* Integration test state and history rollback together
* Load-test order updates and indexed reads; review partitioning only when measured

## Pros and Cons of the Options

### PostgreSQL with current-state and append-only history tables

* Good, because aCID transactions for state changes and history
* Good, because straightforward current-state reads
* Bad, because write throughput remains bounded by the primary; requires indexing and monitoring

### Document-oriented NoSQL with mutable order documents

* Good, because flexible document model and potential horizontal scale
* Bad, because atomic cross-record audit/history workflows can be more involved

### Full event sourcing

* Good, because complete event-centric record of changes
* Bad, because projection, replay, and schema evolution complexity

## More Information

* Scope: architectural proposal for the **I’ll Have the BLT** Kata; no deployed implementation is claimed.
* Related: [ADR 0002](0002-process-order-events-asynchronously.md), [ADR 0004](0004-separate-order-and-payment-state-machines.md).
