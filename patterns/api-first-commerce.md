# API-First Commerce

An API-first commerce architecture treats business capabilities as reusable services rather than capabilities belonging exclusively to a single digital channel.

The objective is not simply to expose APIs.

The objective is to establish **stable contracts between experiences, commerce capabilities, and surrounding enterprise systems.**

---

# 1. The Core Idea

A traditional channel-centric model can evolve into duplicated business logic:

```text
Web
 └── Business Logic

Mobile
 └── Business Logic

B2B Portal
 └── Business Logic
```

An API-first approach moves reusable commerce capabilities behind defined interfaces:

```text
Web ──────────┐
Mobile ───────┤
B2B Portal ───┼──→ Commerce APIs → Commerce Capabilities
Partner ──────┤
Other Channel ┘
```

The experience becomes a consumer of commerce capabilities rather than the owner of those capabilities.

---

# 2. Why API-First Matters

A well-designed API boundary can provide:

* channel independence
* reusable capabilities
* controlled access
* clearer ownership
* easier integration
* consistent business behavior
* improved evolution of digital experiences

It also makes it possible to introduce new channels without rebuilding core commerce functionality.

---

# 3. Capability-Oriented APIs

APIs should expose meaningful business capabilities.

For example:

```text
Product
Pricing
Cart
Checkout
Order
Customer
Inventory
```

The API should represent the business capability rather than simply exposing database structures.

Avoid treating the database schema as the API contract.

---

# 4. API Boundary

A useful conceptual model is:

```text
Experience
    │
    ▼
API Contract
    │
    ▼
Commerce Capability
    │
    ▼
Business Rules / Data
```

The API contract becomes a boundary between the caller and the implementation.

This allows internal implementation to evolve without necessarily requiring every consumer to change.

---

# 5. Avoid Chatty APIs

An API-first architecture can still perform poorly if a single customer interaction requires excessive calls.

For example:

```text
Page
 ├── Product API
 ├── Price API
 ├── Inventory API
 ├── Promotion API
 ├── Customer API
 ├── Recommendation API
 └── Content API
```

This may create unnecessary latency.

API design should consider:

* aggregation
* payload design
* batching
* caching
* request parallelization
* data locality

The goal is not "more APIs."

The goal is **useful and efficient boundaries**.

---

# 6. Contract Design

A good API contract should clearly define:

* request structure
* response structure
* required fields
* optional fields
* error behavior
* authorization requirements
* versioning strategy
* compatibility expectations

Contracts should be designed for consumers rather than exposing internal implementation details.

---

# 7. Error Handling

Errors should be predictable.

A useful conceptual model:

```text
Request
   │
   ├── Valid → Process
   │
   ├── Invalid → Validation Error
   │
   ├── Unauthorized → Authorization Error
   │
   └── Dependency Failure → Recoverable / Non-Recoverable Error
```

Consumers should be able to distinguish between errors they can correct and failures that require retry or operational intervention.

---

# 8. Security

API-first architecture requires explicit security boundaries.

Consider:

* authentication
* authorization
* scope
* rate limiting
* input validation
* sensitive data exposure
* audit requirements

A valid authenticated user should not automatically have access to every commerce capability.

Authorization should reflect business context.

---

# 9. Versioning

APIs evolve.

Potential strategies include:

* backward-compatible evolution
* explicit versions
* deprecation periods
* contract testing

Versioning should be treated as a lifecycle concern rather than something introduced only after breaking consumers.

---

# 10. API vs Event

APIs and events solve different problems.

| API                       | Event                                   |
| ------------------------- | --------------------------------------- |
| Request/response          | Notification of a fact                  |
| Caller expects a result   | Consumers react independently           |
| Often synchronous         | Usually asynchronous                    |
| Tighter temporal coupling | Lower temporal coupling                 |
| Good for queries/actions  | Good for state changes and side effects |

They can coexist in the same architecture.

---

# 11. API Design Checklist

Before introducing an API:

```text
[ ] What business capability does it represent?
[ ] Who owns the capability?
[ ] Who consumes it?
[ ] Is the contract stable?
[ ] What authorization is required?
[ ] What happens when dependencies fail?
[ ] Is the payload appropriate?
[ ] Can repeated calls be avoided?
[ ] Is caching possible?
[ ] Is versioning required?
[ ] Can the API be observed?
```

---

# Final Principle

> **An API should expose a capability, not an implementation.**

Good API-first architecture creates stable business boundaries that allow digital experiences and enterprise systems to evolve independently.
