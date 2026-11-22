---
status: accepted
date: 2026-11-22
decision-makers: ["Kata solution author"]
---

# Use a Shared Notification Capability

## Context and Problem Statement

Clients, restaurants and drivers must receive updates on orders and dispatch. Sending push, email, or other notifications directly from each business service duplicates channel integrations.

## Decision Drivers

* Support multiple recipient types and channels
* Keep communication delivery separate from domain decisions
* Avoid blocking order operations on notification providers

## Considered Options

* Shared Notifications Service consuming notification requests/events
* Embed notification logic in every domain service
* Send notification inline during order transaction

## Decision Outcome

Chosen option: **"Shared Notifications Service consuming notification requests/events"**, because it best satisfies the decision drivers for this Kata without adding complexity that the available requirements do not justify.

### Consequences

* Domain services decide when and whom to notify; Notifications Service executes channel delivery.
* Use push for mobile drivers; a webhook is appropriate for server-to-server partner integrations, not a direct push substitute.
* Notification failure does not roll back a successfully committed order.

### Confirmation

* Simulate notification provider outage while new orders continue
* Verify recipient routing and deduplication on duplicate messages

## Pros and Cons of the Options

### Shared Notifications Service consuming notification requests/events

* Good, because single place for channel integrations and retry policy
* Good, because enables asynchronous delivery
* Bad, because requires clear notification contracts and recipient routing

### Embed notification logic in every domain service

* Good, because fewer initial services
* Bad, because duplicate integrations and inconsistent retry behavior

### Send notification inline during order transaction

* Good, because easy sequential flow
* Bad, because provider latency/failure can block ordering

## More Information

* Scope: architectural proposal for the **I’ll Have the BLT** Kata; no deployed implementation is claimed.
* Related: [ADR 0008](0008-separate-delivery-dispatch-from-ordering.md).
