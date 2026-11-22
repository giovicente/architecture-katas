---
status: accepted
date: 2026-11-22
decision-makers: ["Kata solution author"]
---

# Expire Unconfirmed Orders Using a Scheduled Worker

## Context and Problem Statement

Restaurants can fail to accept or reject an order, leaving it indefinitely awaiting confirmation. The Kata needs a bounded deadline and eventual cancellation/compensation.

## Decision Drivers

* Avoid permanently stuck orders
* Reuse Ordering state-transition rules
* Keep scheduling operationally simple

## Considered Options

* Scheduled worker scans confirmationDeadline and signals Orders Processor
* Delayed broker messages per order
* No explicit timeout

## Decision Outcome

Chosen option: **"Scheduled worker scans confirmationDeadline and signals Orders Processor"**, because it best satisfies the decision drivers for this Kata without adding complexity that the available requirements do not justify.

### Consequences

* Record confirmationDeadline on orders; start with a worker cadence of approximately 30 seconds, subject to the business SLA.
* The worker emits a deadline-reached signal, not an unconditional expire command; the processor expires only if status is still AWAITING_RESTAURANT_CONFIRMATION and the deadline has passed.
* If already paid, expiry triggers a refund workflow; late restaurant acceptance cannot revive the expired order.

### Confirmation

* Race-test acceptance at the deadline
* Monitor order_expiration_lag and indexed query cost under load

## Pros and Cons of the Options

### Scheduled worker scans confirmationDeadline and signals Orders Processor

* Good, because simple PostgreSQL-based recovery
* Good, because allows conditional expiration and easy monitoring
* Bad, because polling load and up to one interval of extra lag in healthy conditions

### Delayed broker messages per order

* Good, because avoids periodic scans
* Bad, because depends on scheduling infrastructure and reliable delayed delivery

### No explicit timeout

* Good, because no extra worker
* Bad, because orders can remain unresolved indefinitely

## More Information

* Scope: architectural proposal for the **I’ll Have the BLT** Kata; no deployed implementation is claimed.
* Related: [ADR 0004](0004-separate-order-and-payment-state-machines.md), [ADR 0010](0010-handle-refunds-asynchronously.md).
