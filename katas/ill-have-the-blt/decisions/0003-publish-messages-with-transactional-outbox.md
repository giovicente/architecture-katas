---
status: accepted
date: 2026-11-22
decision-makers: ["Kata solution author"]
---

# Publish Messages with Transactional Outbox

## Context and Problem Statement

Writing an order and sending its event are separate operations. A crash between them can leave a persisted order without a downstream event. The same risk applies when a processor expires an order and requests a refund.

## Decision Drivers

* Do not lose business events after committed database changes
* Avoid distributed transactions with the broker
* Support recoverable at-least-once delivery

## Considered Options

* Transactional Outbox in Ordering PostgreSQL
* Direct publish after database commit
* Distributed transaction across DB and broker

## Decision Outcome

Chosen option: **"Transactional Outbox in Ordering PostgreSQL"**, because it best satisfies the decision drivers for this Kata without adding complexity that the available requirements do not justify.

### Consequences

* Write order/status changes and outbox entries in one transaction.
* A separate relay publishes and marks entries delivered; consumers remain idempotent.
* Use stable message IDs and monitor relay lag.

### Confirmation

* Fault-injection test crash after commit/before publish
* Verify redelivery does not duplicate order transitions or refunds

## Pros and Cons of the Options

### Transactional Outbox in Ordering PostgreSQL

* Good, because business change and outgoing message commit together
* Good, because publisher can retry after broker outages
* Bad, because additional table, relay and cleanup; duplicate deliveries remain possible

### Direct publish after database commit

* Good, because fewest components
* Bad, because crash window can lose a message

### Distributed transaction across DB and broker

* Good, because potential cross-system atomicity
* Bad, because operational complexity and poor compatibility

## More Information

* Scope: architectural proposal for the **I’ll Have the BLT** Kata; no deployed implementation is claimed.
* Related: [ADR 0002](0002-process-order-events-asynchronously.md), [ADR 0010](0010-handle-refunds-asynchronously.md).
