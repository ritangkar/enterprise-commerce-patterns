# Integration Architecture

Enterprise commerce rarely operates as an isolated application.

A typical landscape may include:

```text
Commerce
 ├── ERP
 ├── CRM
 ├── Customer Service
 ├── PIM
 ├── Payment
 ├── Inventory
 ├── Tax
 ├── Search
 ├── Notifications
 └── External Partners
```

Integration architecture defines how these systems exchange capabilities and information without creating unnecessary coupling.

---

## 1. Integration Principles

A strong integration architecture should provide:

- Clear ownership
- Explicit contracts
- Appropriate communication patterns
- Failure isolation
- Observability
- Security
- Idempotency
- Versioning

---

## 2. API vs Event

APIs are generally appropriate when a consumer needs a response to a request.

```text
Consumer
   ↓ Request
Provider
   ↓ Response
Consumer
```

Events are appropriate when a system needs to communicate that something happened.

```text
Producer
   ↓
Business Event
   ↓
Multiple Consumers
```

The choice should be based on business semantics rather than technology preference.

---

## 3. Synchronous Integration

Useful when the caller needs an immediate answer.

Examples:

- Price lookup
- Availability validation
- Customer authentication
- Delivery options

Risks include:

- Latency
- Availability coupling
- Cascading failures

---

## 4. Asynchronous Integration

Useful for processes that can complete independently.

Examples:

- Notifications
- Search indexing
- Analytics
- Secondary system synchronization
- Non-critical downstream processing

Benefits include:

- Decoupling
- Buffering
- Independent scaling
- Better failure isolation

---

## 5. Integration Contracts

Contracts should define:

- Request / event structure
- Required fields
- Optional fields
- Validation
- Error behavior
- Versioning
- Compatibility expectations

Avoid exposing internal database structures as integration contracts.

---

## 6. Canonical Models

A canonical model can reduce translation complexity in some enterprise environments.

However, excessive canonical modeling can also create a centralized abstraction that satisfies nobody.

Use canonical models when they genuinely reduce complexity.

Avoid introducing them purely because multiple systems exist.

---

## 7. Integration Ownership

For each important piece of information, define:

```text
Who owns it?
Who publishes it?
Who consumes it?
Who can modify it?
What happens if synchronization fails?
```

Example:

```text
Product Data
   ↓
Authoritative Source
   ↓
Integration
   ↓
Commerce
   ↓
Search
```

Ownership should remain unambiguous.

---

## 8. Data Synchronization

Synchronization may be:

- Real-time
- Near-real-time
- Scheduled
- Batch
- Event-driven

The appropriate choice depends on business freshness requirements.

Not every piece of data requires real-time synchronization.

---

## 9. Integration Failures

A robust integration should account for:

- Timeouts
- Duplicate messages
- Out-of-order events
- Schema changes
- Authentication failures
- Partial processing
- Downstream outages

Recovery mechanisms should be designed as part of the integration rather than added after the first incident.

---

## 10. Integration Layer

An integration layer can provide:

- Protocol translation
- Transformation
- Routing
- Authentication
- Error handling
- Monitoring

However, it should not become a dumping ground for business logic.

Business ownership should remain close to the domain that owns the capability.

---

## 11. Common Failure Modes

### Point-to-point explosion

```text
System A ↔ B
System A ↔ C
System A ↔ D
System B ↔ C
...
```

**Impact:** Increasing integration complexity.

### Shared database integration

**Problem:** Systems integrate by directly reading or writing another system's database.

**Impact:** Tight coupling and unclear ownership.

### Everything is synchronous

**Problem:** Every downstream dependency must respond immediately.

**Impact:** Fragility and latency.

### Integration layer becomes business engine

**Problem:** Business rules accumulate inside middleware.

**Impact:** Domain ownership becomes unclear.

---

## 12. Architectural Principle

> **Integration should connect business capabilities without transferring ownership of those capabilities.**