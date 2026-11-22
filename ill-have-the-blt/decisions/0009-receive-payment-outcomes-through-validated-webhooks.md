---
status: accepted
date: 2026-11-22
decision-makers: ["Kata solution author"]
---

# Receive Payment Outcomes through Validated Webhooks

## Context and Problem Statement

Online payments and refunds can complete asynchronously. The mobile app response is not sufficient proof that the payment gateway settled a transaction.

## Decision Drivers

* Obtain authoritative external payment outcomes
* Avoid exposing the processor as a public HTTP service
* Support replay and duplicate webhook delivery

## Considered Options

* Payment gateway webhook terminates at Orders API, then broker/processor
* Gateway calls Orders Processor directly
* Trust immediate client payment callback

## Decision Outcome

Chosen option: **"Payment gateway webhook terminates at Orders API, then broker/processor"**, because it best satisfies the decision drivers for this Kata without adding complexity that the available requirements do not justify.

### Consequences

* Orders API verifies webhook signature/authenticity, provider transaction identity, amount and correspondence to the order.
* Persist incoming notification durably before acknowledging; publish normalized financial outcome through the broker.
* Orders Processor updates paymentStatus only after validated outcomes; reconcile unknown/late results with the gateway.

### Confirmation

* Replay duplicate and out-of-order webhooks
* Fault-injection test broker outage after webhook receipt

## Pros and Cons of the Options

### Payment gateway webhook terminates at Orders API, then broker/processor

* Good, because keeps external HTTP entrypoint distinct from asynchronous business processing
* Good, because supports durable event receipt and validation
* Bad, because requires webhook authentication, inbox/outbox and reconciliation

### Gateway calls Orders Processor directly

* Good, because fewer logical hops
* Bad, because exposes a worker-oriented component and mixes transport concerns

### Trust immediate client payment callback

* Good, because simple UI integration
* Bad, because unreliable and forgeable financial confirmation

## More Information

* Scope: architectural proposal for the **I’ll Have the BLT** Kata; no deployed implementation is claimed.
* Related: [ADR 0004](0004-separate-order-and-payment-state-machines.md), [ADR 0010](0010-handle-refunds-asynchronously.md).
