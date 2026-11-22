---
status: accepted
date: 2026-11-22
decision-makers: ["Kata solution author"]
---

# Compose Catalog from Independent Item and Offer Services

## Context and Problem Statement

Available sandwiches and promotions vary by store/region. A direct Items API to Offers API call would require catalog inventory logic to know the customer location and promotion eligibility.

## Decision Drivers

* Keep product and promotion ownership separate
* Provide a convenient mobile-facing read model
* Minimize unnecessary customer address propagation

## Considered Options

* Catalog API composes Items API, Offers API and store context
* Items API calls Offers API
* Client calls every service separately

## Decision Outcome

Chosen option: **"Catalog API composes Items API, Offers API and store context"**, because it best satisfies the decision drivers for this Kata without adding complexity that the available requirements do not justify.

### Consequences

* Catalog API returns item data plus offer previews for a selected store or commercial region.
* The Orders API recomputes/validates effective prices and eligibility before accepting an order.
* Catalog composition may live as a BFF rather than a standalone deployable microservice.

### Confirmation

* Contract test store-specific offer composition
* Verify expired or tampered promotional pricing is rejected at checkout

## Pros and Cons of the Options

### Catalog API composes Items API, Offers API and store context

* Good, because central composition contract
* Good, because items remains independent of offers
* Bad, because adds a read-side component and fan-out latency

### Items API calls Offers API

* Good, because quick initial implementation
* Bad, because couples catalog data to promotions and locale

### Client calls every service separately

* Good, because no server-side composer
* Bad, because more mobile requests, business composition leaks to clients

## More Information

* Scope: architectural proposal for the **I’ll Have the BLT** Kata; no deployed implementation is claimed.
* Related: [ADR 0006](0006-key-promotions-by-store-or-region.md), [ADR 0012](0012-scale-reads-and-isolate-external-failures.md).
