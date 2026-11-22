---
status: accepted
date: 2026-11-22
decision-makers: ["Kata solution author"]
---

# Separate Order and Payment State Machines

## Context and Problem Statement

The Kata accepts online payment, payment at pickup and payment on delivery. Preparation can proceed while a payment is pending; cancellation and refund can happen independently of order fulfillment.

## Decision Drivers

* Represent independent business lifecycles accurately
* Prevent invalid transitions under delayed events
* Make payment requirements explicit at pickup/delivery

## Considered Options

* Separate orderStatus and paymentStatus with guarded cross-domain invariants
* Single combined status field
* Use payment events without persisted financial state

## Decision Outcome

Chosen option: **"Separate orderStatus and paymentStatus with guarded cross-domain invariants"**, because it best satisfies the decision drivers for this Kata without adding complexity that the available requirements do not justify.

### Consequences

* Order states include CREATED, AWAITING_RESTAURANT_CONFIRMATION, CONFIRMED, PREPARING, READY, PICKED_UP, OUT_FOR_DELIVERY, DELIVERED, CANCELLED and EXPIRED.
* Payment states include PENDING, PROCESSING, AWAITING_IN_PERSON_PAYMENT, PAID, DECLINED, REFUND_PENDING, REFUNDED and REFUND_FAILED, as applicable.
* Pickup/delivery completion checks payment settlement according to the selected payment method and business policy. Exact rules for cash and disputed payments remain business assumptions.

### Confirmation

* Model valid and invalid transitions as table-driven tests
* Simulate late PaymentApproved after EXPIRED; verify no order reactivation

## Pros and Cons of the Options

### Separate orderStatus and paymentStatus with guarded cross-domain invariants

* Good, because keeps each state machine cohesive
* Good, because supports refund after order cancellation
* Bad, because requires coordinated checks at fulfillment boundaries

### Single combined status field

* Good, because only one property to query
* Bad, because combinatorial state explosion and ambiguous names

### Use payment events without persisted financial state

* Good, because fewer persisted fields
* Bad, because harder to query current payment disposition reliably

## More Information

* Scope: architectural proposal for the **I’ll Have the BLT** Kata; no deployed implementation is claimed.
* Delivery assignment/status is a related but separate concern; see [ADR 0008](0008-separate-delivery-dispatch-from-ordering.md).
