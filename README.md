# model-gateway

**Provider-neutral model invocation and resource management for FlossWare.**

This repository is the successor to `model-router`. Routing is an implementation concern of the gateway, not the gateway's architectural identity.

## Boundary

```text
Worker / Arbiter
      │
      ▼
Model Gateway
  ├── providers
  ├── models
  ├── resources/accounts
  ├── credentials
  ├── hard feasibility checks
  ├── selection strategies
  ├── invocation
  ├── usage/cost
  └── provenance
```

The Gateway exposes a stable provider-neutral contract. Providers and resources are interchangeable implementations. A Worker should not know provider-specific credentials or routing mechanics.

## Selection order

```text
request
  ↓
candidates
  ↓
hard constraints
  ↓
feasible resources/models
  ↓
selection strategy
  ↓
invocation
  ↓
usage / cost / latency / outcome
  ↓
learning and evaluation
```

Authorization, policy, capability, budget, quota, rate, and availability constraints are authoritative. Adaptive strategies such as Thompson Sampling may improve selection only after the feasible set is established.

## Strategies

Deterministic and adaptive strategies can be supplied independently, including round-robin, weighted selection, least-utilized, least-cost, Thompson Sampling, UCB, and other implementations of the FlossWare strategy contracts.

The gateway does not own the implementation of those algorithms. It composes them.

## Credentials and provenance

Credentials remain inside the gateway/resource boundary. They must never appear in Worker configuration, Intent, evidence, logs, provenance records, or MCP responses.

Selection and invocation preserve non-secret provenance such as provider, resource, model, strategy/version, applicable constraints, usage, cost, latency, and outcome.

## Prompt caching

Provider-native prompt caching is a gateway capability. Cache eligibility, identity, invalidation, sharing scope, and observability are provider-neutral concerns; unsupported providers fall back without changing Worker contracts.

## Relationship to Loom

`loom-ai` owns Worker/Arbiter execution and Intent. `model-gateway` supplies model capability to that runtime. The gateway can also be consumed by other compatible clients without making those clients part of its architecture.

## Migration

The former `model-router` terminology should be treated as historical. New APIs, documentation, and dependencies should use `model-gateway` and describe routing/selection as replaceable strategies or implementations.

## License

MIT
