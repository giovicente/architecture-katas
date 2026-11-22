---
status: accepted
date: 2026-11-22
decision-makers: ["Kata solution author"]
---

# Evaluate Promotions by Store or Commercial Region

## Context and Problem Statement

National and local daily specials coexist. A customer may order for pickup at a store outside their home city, so the personal delivery address is not always the correct promotion scope.

## Decision Drivers

* Support national and local promotions
* Avoid using full personal addresses where unnecessary
* Make offer eligibility repeatable at checkout

## Considered Options

* Use storeId or regionId as eligibility context
* Use full customer address
* Apply only national promotions

## Decision Outcome

Chosen option: **"Use storeId or regionId as eligibility context"**, because it best satisfies the decision drivers for this Kata without adding complexity that the available requirements do not justify.

### Consequences

* Prefer storeId or regionId for offers and catalog queries; model national, region and optionally store scopes.
* Store the promotion validity window and revalidate eligibility on order confirmation.
* If delivery-location-specific promotion rules emerge, document their precedence separately.

### Confirmation

* Test pickup in a different city and nationwide versus local offers
* Test boundary times around promotional expiry

## Pros and Cons of the Options

### Use storeId or regionId as eligibility context

* Good, because stable commercial scope
* Good, because less customer personal data passed between services
* Bad, because requires explicit store-to-region mapping

### Use full customer address

* Good, because useful when promos follow delivery area
* Bad, because more coupling and sensitive data propagation

### Apply only national promotions

* Good, because simplest eligibility
* Bad, because fails the local-promotions requirement

## More Information

* Scope: architectural proposal for the **I’ll Have the BLT** Kata; no deployed implementation is claimed.
* Related: [ADR 0005](0005-compose-catalog-with-independent-item-and-offer-services.md).
