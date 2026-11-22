---
status: accepted
date: 2026-11-22
decision-makers: ["Kata solution author"]
---

# Scale Read Workloads and Isolate External Failures

## Context and Problem Statement

Millions of users do not imply millions of concurrent checkouts. Catalog reads may substantially exceed order writes; Maps and payment providers impose their own quotas and failure modes. Capacity figures and SLOs are not specified in the Kata.

## Decision Drivers

* Scale from measured load without premature sharding
* Protect order ingestion against dependency failures
* Reduce repeated catalog and promotion reads

## Considered Options

* Stateless API scaling, cache for catalog, bounded retries and circuit breakers on external clients
* Make all calls synchronous with no isolation
* Shard databases and adopt NoSQL immediately

## Decision Outcome

Chosen option: **"Stateless API scaling, cache for catalog, bounded retries and circuit breakers on external clients"**, because it best satisfies the decision drivers for this Kata without adding complexity that the available requirements do not justify.

### Consequences

* Put public APIs behind a gateway/ingress with load balancing and rate limiting; horizontally scale stateless services and broker consumers.
* Use cache for store, item and offer read models; revalidate price on order submission.
* External calls use timeouts, bounded exponential backoff with jitter, circuit breakers and provider-specific fallback. Payment/refund ambiguous results require reconciliation and idempotency, never naive replay.
* Monitor queue lag, dead letters, database write latency, webhook lag, expiration lag and provider error rates; tune capacity after load tests.

### Confirmation

* Define and run representative peak-load tests before sizing
* Inject Maps failures and payment timeouts; assert order creation remains consistent

## Pros and Cons of the Options

### Stateless API scaling, cache for catalog, bounded retries and circuit breakers on external clients

* Good, because independent scaling of hot APIs
* Good, because resilience against temporary external incidents
* Bad, because requires cache invalidation, fallback semantics and telemetry

### Make all calls synchronous with no isolation

* Good, because simple to prototype
* Bad, because risk of cascading latency and provider amplification

### Shard databases and adopt NoSQL immediately

* Good, because potential long-term scale
* Bad, because adds complexity without evidence of the bottleneck

## More Information

* Scope: architectural proposal for the **I’ll Have the BLT** Kata; no deployed implementation is claimed.
* Read replicas are for read scale, not increased primary write throughput. Runtime policies are proposals pending measured SLOs.
