# Commerce Architecture Anti-Patterns

Architecture anti-patterns are recurring approaches that appear convenient but create long-term problems.

Recognizing them is useful because enterprise commerce systems often evolve incrementally.

A system rarely becomes complex through one catastrophic decision.

It usually accumulates complexity through many locally convenient decisions.

---

## 1. The Customization Trap

```text id="x4ub23"
Business Requirement
       ↓
Custom Code
       ↓
Another Requirement
       ↓
More Custom Code
       ↓
Upgrade Complexity
```

### Symptoms

- Large customizations
- Difficult upgrades
- Duplicate platform functionality
- Regression risk

### Better Approach

Evaluate:

```text
Configuration
→ Extension
→ Integration
→ Customization
```

in that order where appropriate.

---

## 2. The God Service

One service becomes responsible for:

- Product
- Customer
- Pricing
- Cart
- Order
- Payment
- Fulfillment

### Symptoms

- Large codebase
- High deployment risk
- Difficult ownership
- Slow changes

The answer is not automatically “microservices.”

The underlying problem is unclear responsibility.

---

## 3. Distributed Monolith

A system may technically contain many services while remaining tightly coupled.

Example:

```text id="1m6c7n"
Service A
 ↓ synchronous
Service B
 ↓ synchronous
Service C
 ↓ synchronous
Service D
```

If all services must be available for one request, the system may still behave like a monolith operationally.

---

## 4. Shared Database Coupling

Multiple systems directly modify the same database.

```text id="n7w4h2"
System A ─┐
System B ─┼→ Shared Database
System C ─┘
```

This creates:

- Hidden dependencies
- Schema coupling
- Ownership ambiguity
- Difficult independent evolution

---

## 5. Chatty Integration

A single business operation generates many remote calls.

```text id="5a7e9s"
Request
 ├── API A
 ├── API B
 ├── API C
 ├── API D
 ├── API E
 └── API F
```

Consequences:

- Higher latency
- More failure points
- Greater network overhead

Prefer capability-oriented APIs and appropriate aggregation.

---

## 6. Synchronous Everything

Not every operation requires an immediate response.

Examples that may often be asynchronous:

- Notifications
- Analytics
- Search indexing
- Secondary synchronization

Synchronous communication should be reserved for interactions that genuinely require immediate results.

---

## 7. Cache Without Ownership

A cache is introduced without answering:

- Who invalidates it?
- How stale can it become?
- What happens during failure?
- Which version is authoritative?

A cache without an invalidation strategy becomes a source of incorrect behavior.

---

## 8. Business Logic in Middleware

Integration middleware gradually accumulates:

- Pricing rules
- Customer rules
- Promotion rules
- Order rules

This creates unclear ownership.

Middleware should generally connect and transform capabilities rather than become the hidden business domain.

---

## 9. UI-Owned Business Logic

Important rules implemented only in the frontend can be bypassed by:

- APIs
- Other channels
- Integrations
- Administrative tools

Business-critical validation must exist at appropriate backend capability boundaries.

---

## 10. The Happy-Path Architecture

A design that only considers:

```text id="l3w1cn"
Success → Success → Success
```

is incomplete.

Enterprise architecture should also define:

```text id="a9m2u7"
Timeout
Retry
Duplicate
Partial Failure
Unknown Outcome
Recovery
Reconciliation
```

---

## 11. Technology-First Architecture

Starting with:

> “Which technology should we use?”

can obscure the actual problem.

A stronger sequence is:

```text id="k6xw0j"
Business Problem
      ↓
Capability
      ↓
Constraints
      ↓
Architecture
      ↓
Technology
```

---

## 12. Common Failure Pattern

The most dangerous anti-pattern is often not a specific technology.

It is:

> **Allowing temporary implementation decisions to become permanent architecture without deliberate review.**

---

## 13. Architectural Principle

> **Avoid complexity that does not create corresponding business or technical value.**