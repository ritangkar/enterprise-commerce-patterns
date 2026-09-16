# Scalability

Enterprise commerce platforms must support changing demand without requiring proportional increases in system complexity or operational effort.

Scalability is not simply adding more servers.

It is the ability to handle increasing:

- Traffic
- Transactions
- Catalog size
- Customer volume
- Integrations
- Data
- Geographic coverage

while maintaining acceptable performance and reliability.

---

## 1. Scaling Dimensions

A commerce platform may need to scale across multiple dimensions:

```text
                 Scalability
                     │
       ┌─────────────┼─────────────┐
       ↓             ↓             ↓
   Traffic         Data         Transactions
       │             │             │
    Users         Catalog         Orders
```

Different bottlenecks require different solutions.

---

## 2. Horizontal vs Vertical Scaling

### Vertical scaling

Increase the capacity of an existing node.

```text
Small Node
   ↓
Larger Node
```

### Horizontal scaling

Add additional nodes.

```text
        Load Balancer
        /     |     \
    Node A  Node B  Node C
```

Stateless application services generally make horizontal scaling easier.

---

## 3. Stateless Services

Where possible, request processing should not depend on local server memory.

Instead:

```text
Request
  ↓
Any Application Node
  ↓
Shared / Durable State
```

This improves:

- Horizontal scaling
- Failover
- Deployment flexibility
- Infrastructure utilization

Stateful components require deliberate architecture rather than accidental local state.

---

## 4. Database Scalability

Databases are frequently a major commerce bottleneck.

Important considerations include:

- Data modeling
- Index design
- Query efficiency
- Connection management
- Read/write patterns
- Transaction boundaries
- Partitioning where justified
- Archival strategies

Adding application nodes does not solve an inefficient database query.

---

## 5. Caching

Caching can reduce repeated expensive operations.

Potential cache targets include:

- Product information
- Configuration
- Reference data
- Search results
- Pricing where safely cacheable
- API responses

Caching should be designed alongside invalidation.

> A fast incorrect value is still incorrect.

---

## 6. Asynchronous Processing

Long-running or non-critical work can often be moved away from the synchronous request path.

For example:

```text
Customer Request
      ↓
Critical Transaction
      ↓
Immediate Response

      +

Asynchronous Event
      ↓
Notifications
Analytics
Indexing
Secondary Integrations
```

This can reduce customer-facing latency.

---

## 7. Traffic Spikes

Commerce traffic may be highly uneven.

Examples include:

- Campaign launches
- Product releases
- Seasonal demand
- Promotions
- Flash sales

Architecture should consider:

- Autoscaling
- Queue buffering
- Rate limiting
- Cache warming
- Capacity planning
- Graceful degradation

---

## 8. Search Scalability

Search workloads can become expensive with:

- Large catalogs
- Complex facets
- Personalized results
- High query volume

Search infrastructure should be independently scalable from transactional commerce services where appropriate.

---

## 9. Resilience vs Scalability

Scalability and resilience are related but different.

```text
Scalability
→ Can the system handle more demand?

Resilience
→ Can the system continue operating when something fails?
```

A scalable system can still be fragile.

---

## 10. Common Failure Modes

### Scaling the wrong layer

**Problem:** Application nodes are increased while the database remains the bottleneck.

**Impact:** Little or no improvement.

### Excessive synchronous processing

**Problem:** Every operation waits for multiple downstream systems.

**Impact:** Latency increases with dependency count.

### Unbounded concurrency

**Problem:** Scaling increases pressure on downstream dependencies.

**Impact:** Cascading failures.

### Cache without invalidation

**Problem:** Cached data becomes stale indefinitely.

**Impact:** Incorrect customer experiences.

---

## 11. Architectural Principle

> **Scale the bottleneck, not the architecture that happens to be easiest to duplicate.**