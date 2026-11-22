---
status: accepted
date: 2026-11-22
decision-makers: ["Kata solution author"]
---

# Handle Refunds Asynchronously

## Context and Problem Statement

Order expiry and customer refund requests can require a remote financial operation that may fail, time out, or complete later. Blocking the Orders Processor on the gateway would couple order throughput to financial provider availability.

## Decision Drivers

* Isolate gateway latency and outages
* Avoid duplicate refunds
* Unify timeout-triggered and customer-requested refund execution

## Considered Options

* Orders Processor issues an asynchronous refund command to Refund Handler
* Orders Processor calls Refund Handler synchronously
* Orders API calls gateway directly on customer click

## Decision Outcome

Chosen option: **"Orders Processor issues an asynchronous refund command to Refund Handler"**, because it best satisfies the decision drivers for this Kata without adding complexity that the available requirements do not justify.

### Consequences

* Orders API durably records customer refund requests; Orders Processor decides eligibility and, when approved, stores REFUND_PENDING and a refund command atomically via outbox.
* Refund Handler calls the gateway with a stable idempotency key and persists transaction correlation.
* Only a validated provider result transitions payment to REFUNDED; unknown results require reconciliation, not blind retry.

### Confirmation

* Simulate duplicate command, gateway timeout after successful processing and late confirmation
* Ensure exactly one intended business refund operation (even if messages deliver at least once)

## Pros and Cons of the Options

### Orders Processor issues an asynchronous refund command to Refund Handler

* Good, because separates eligibility from external execution
* Good, because supports retries, independent scaling and webhook completion
* Bad, because more state transitions and messaging

### Orders Processor calls Refund Handler synchronously

* Good, because direct control flow
* Bad, because processor becomes exposed to remote latency and failure

### Orders API calls gateway directly on customer click

* Good, because immediate flow
* Bad, because bypasses centralized eligibility rules and makes retries harder

## More Information

* Scope: architectural proposal for the **I’ll Have the BLT** Kata; no deployed implementation is claimed.
* Related: [ADR 0003](0003-publish-messages-with-transactional-outbox.md), [ADR 0009](0009-receive-payment-outcomes-through-validated-webhooks.md).
