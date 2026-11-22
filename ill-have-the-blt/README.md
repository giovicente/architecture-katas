# I'll have the BLT

## Description as proposed in Architectural Katas

A national sandwich shop wants to enable "fax in your order" but over the Internet instead (in addition to their current fax-in service)

Users: millions+

Requirements: users will place their order, then be given a time to pick up their sandwich and directions to the shop (which must integrate with Google Maps); if the shop offers a delivery service, dispatch the driver with the sandwich to the user; mobile-device accessibility; offer national daily promotionals/specials; offer local daily promotionals/specials; accept payment online or in person/on delivery

## Architecture Decision Records

This directory documents the key decisions from the **I’ll Have the BLT** system-design Kata using the [MADR 4.0](https://adr.github.io/madr/) style.

**Status interpretation:** `accepted` means adopted in the *proposed design*, not deployed or benchmarked in production. All confirmation criteria remain to be validated in a working system.

## Decision index

* [ADR 0001 — Use PostgreSQL for Order Persistence](decisions/0001-use-postgresql-for-order-persistence.md)
* [ADR 0002 — Process Order Events Asynchronously](decisions/0002-process-order-events-asynchronously.md)
* [ADR 0003 — Publish Messages with Transactional Outbox](decisions/0003-publish-messages-with-transactional-outbox.md)
* [ADR 0004 — Separate Order and Payment State Machines](decisions/0004-separate-order-and-payment-state-machines.md)
* [ADR 0005 — Compose Catalog from Independent Item and Offer Services](decisions/0005-compose-catalog-with-independent-item-and-offer-services.md)
* [ADR 0006 — Evaluate Promotions by Store or Commercial Region](decisions/0006-key-promotions-by-store-or-region.md)
* [ADR 0007 — Use a Shared Notification Capability](decisions/0007-use-shared-notification-capability.md)
* [ADR 0008 — Separate Delivery Dispatch from Ordering](decisions/0008-separate-delivery-dispatch-from-ordering.md)
* [ADR 0009 — Receive Payment Outcomes through Validated Webhooks](decisions/0009-receive-payment-outcomes-through-validated-webhooks.md)
* [ADR 0010 — Handle Refunds Asynchronously](decisions/0010-handle-refunds-asynchronously.md)
* [ADR 0011 — Expire Unconfirmed Orders Using a Scheduled Worker](decisions/0011-expire-unconfirmed-orders-using-a-scheduled-worker.md)
* [ADR 0012 — Scale Read Workloads and Isolate External Failures](decisions/0012-scale-reads-and-isolate-external-failures.md)

## Design boundaries

* Ordering owns order state and a PostgreSQL write model; Orders API and Orders Processor are separate runtime entrypoints within that logical boundary.
* Payment and delivery events are integrated asynchronously, while external HTTP webhooks enter through Orders API.
* Detailed retry parameters, service-level objectives, business policies and peak-load measurements are not specified by the Kata; numerical defaults mentioned in ADRs are initial hypotheses.
* The diagrams and ADRs are proposals open to review, discussion and alternative designs.

## References

* [MADR](https://adr.github.io/madr/)
* [MADR ADR Template](https://adr.github.io/madr/decisions/adr-template)
