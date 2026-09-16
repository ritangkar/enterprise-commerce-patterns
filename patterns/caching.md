# Caching in Commerce Systems

Caching reduces repeated computation, database access, network calls, and expensive processing by keeping frequently required information closer to the consumer.

In commerce systems, caching can significantly improve responsiveness and reduce load.

But caching is not free.

It introduces questions around **freshness, invalidation, consistency, memory, and failure behavior.**

---

# 1. Basic Cache Flow

```text
Request
   │
   ▼
Cache
   │
   ├── Hit ─────→ Return cached value
   │
   └── Miss
        │
        ▼
     Source
        │
        ▼
     Update Cache
        │
        ▼
     Return Value
```

---

# 2. What Should Be Cached?

Potential candidates include:

* relatively stable product information
* reference data
* configuration
* frequently accessed content
* search results
* derived information
* expensive computations

The decision should depend on:

* access frequency
* computation cost
* data volatility
* acceptable staleness
* memory requirements

---

# 3. Cacheability

A useful question is:

> How expensive is it to obtain this information, and how often does it change?

Conceptually:

```text
High Read Frequency
        +
High Computation Cost
        +
Low Volatility
        ↓
Strong Cache Candidate
```

Highly volatile transactional data may require a different strategy.

---

# 4. Cache Invalidation

The difficult part of caching is often not storing data.

It is knowing when cached data is no longer valid.

Common approaches include:

### Time-based expiration

```text
Data
 ↓
TTL
 ↓
Expiration
```

### Event-based invalidation

```text
Product Updated
      ↓
Invalidate Product Cache
```

### Explicit invalidation

The application removes or refreshes the cache when the underlying state changes.

Each approach has trade-offs.

---

# 5. Staleness

Cached information may become stale.

The acceptable level depends on the business capability.

For example:

```text
Search result
```

may tolerate a short delay.

Whereas:

```text
Payment state
```

may require much stronger consistency.

Therefore:

> **Cache policy should be determined by business consequence, not simply technical convenience.**

---

# 6. Cache Stampede

A cache stampede can occur when many requests attempt to rebuild the same expired value simultaneously.

```text
Cache expires
     │
     ├── Request 1 → Source
     ├── Request 2 → Source
     ├── Request 3 → Source
     ├── Request 4 → Source
     └── Request 5 → Source
```

The sudden load can overwhelm the underlying system.

Possible mitigation strategies include:

* request coalescing
* staggered expiration
* background refresh
* locking
* stale-while-revalidate approaches

The appropriate mechanism depends on the workload.

---

# 7. Cache Layers

Commerce systems may contain multiple caching layers:

```text
Customer
   ↓
CDN / Edge
   ↓
Application Cache
   ↓
Search Cache
   ↓
Database
```

Each layer should have a clearly understood responsibility.

Adding multiple caches without understanding their interaction can make troubleshooting difficult.

---

# 8. Cache Keys

Cache correctness depends heavily on cache keys.

A cache key should represent the inputs that materially affect the result.

For example, a price may depend on:

```text
Product
+
Customer Context
+
Currency
+
Quantity
+
Market
```

If the cache key ignores an important dimension, the system may return an incorrect value.

---

# 9. Caching and Personalization

Personalized information requires additional care.

A response that depends on customer-specific context should not accidentally become available to another customer through an incorrectly scoped cache.

Therefore, cache design should explicitly consider:

* identity
* authorization
* customer context
* market
* language
* currency
* channel

---

# 10. Cache Failure

A cache should not automatically become a single point of failure.

The architecture should define what happens when the cache is unavailable.

Possible behavior:

```text
Cache unavailable
      ↓
Fallback to source
      ↓
Degraded but functional experience
```

Where appropriate.

However, fallback behavior must be evaluated against expected load because suddenly bypassing a cache can overload the underlying system.

---

# 11. Cache Performance Metrics

Useful metrics include:

* hit ratio
* miss ratio
* latency
* eviction rate
* memory usage
* refresh frequency
* stale-data rate
* backend load

A high hit ratio is useful, but it is not the only measure of a successful cache strategy.

---

# 12. Caching Checklist

```text
[ ] What problem does the cache solve?
[ ] How frequently is the data accessed?
[ ] How frequently does it change?
[ ] What level of staleness is acceptable?
[ ] What should the cache key contain?
[ ] How is invalidation handled?
[ ] What happens on cache miss?
[ ] What happens if the cache fails?
[ ] Can cache stampede occur?
[ ] Is personalized data safely isolated?
[ ] What metrics are monitored?
```

---

# Final Principle

> **Cache deliberately. Optimize access patterns first, understand freshness requirements, and design invalidation before declaring a caching strategy complete.**

A fast system that returns incorrect data is not a performant commerce system.
