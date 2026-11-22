---
status: accepted
date: 2026-11-22
decision-makers: ["Kata solution author"]
---

# Separate Delivery Dispatch from Ordering

## Context and Problem Statement

Only some shops offer delivery. An order may be ready while no driver is assigned. A rejected offer should not cancel the whole order by default.

## Decision Drivers

* Keep restaurant preparation independent of driver assignment
* Support retries and reassignment of drivers
* Avoid conflating dispatch and order state

## Considered Options

* Drivers API owns dispatch; Orders Processor reacts to delivery events
* Keep driver status in orderStatus only
* Orders API directly operates driver apps

## Decision Outcome

Chosen option: **"Drivers API owns dispatch; Orders Processor reacts to delivery events"**, because it best satisfies the decision drivers for this Kata without adding complexity that the available requirements do not justify.

### Consequences

* Drivers API manages driver offers, acceptance and re-assignment.
* Driver app receives push; it accepts/declines via Drivers API; resulting events are consumed by Orders Processor.
* READY and driver ASSIGNED are separate prerequisites; OUT_FOR_DELIVERY requires actual physical collection.

### Confirmation

* Test multiple drivers accepting at once
* Test driver rejection and reassignment without re-opening a cancelled order

## Pros and Cons of the Options

### Drivers API owns dispatch; Orders Processor reacts to delivery events

* Good, because clear ownership and independently progressing workflows
* Good, because supports reassignment
* Bad, because requires consistency between delivery and order views

### Keep driver status in orderStatus only

* Good, because one state machine
* Bad, because state explosion and awkward retry semantics

### Orders API directly operates driver apps

* Good, because minimal service count
* Bad, because couples ordering with transport and dispatch

## More Information

* Scope: architectural proposal for the **I’ll Have the BLT** Kata; no deployed implementation is claimed.
* Related: [ADR 0004](0004-separate-order-and-payment-state-machines.md), [ADR 0007](0007-use-shared-notification-capability.md).
