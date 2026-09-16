# Observability

Enterprise commerce systems are distributed systems.

A customer-facing transaction may cross:

```text
Frontend
 ↓
Commerce API
 ↓
Commerce Services
 ↓
Integration Layer
 ↓
ERP / CRM / Payment / Inventory
 ↓
External Provider
```

When something fails, knowing that an error occurred is not enough.

The architecture should make it possible to understand **where, why, and under what conditions** it occurred.

---

## 1. The Three Core Signals

A common observability model uses:

- Metrics
- Logs
- Traces

Together:

```text
                Observability
                     │
        ┌────────────┼────────────┐
        ↓            ↓            ↓
      Metrics       Logs        Traces
```

---

## 2. Metrics

Metrics describe system behavior over time.

Useful commerce metrics include:

- Request rate
- Error rate
- Latency
- Throughput
- CPU / memory utilization
- Queue depth
- Cache hit ratio
- Database latency
- Integration failures
- Checkout conversion
- Order-processing latency

Latency should often be evaluated using percentiles.

For example:

```text
p50 → Typical experience
p95 → Slow-tail experience
p99 → Extreme-tail experience
```

Averages alone can hide serious performance problems.

---

## 3. Structured Logging

Logs should provide useful machine-readable context.

For example:

```text
timestamp
service
operation
correlationId
requestId
entityId
status
duration
errorCode
```

Avoid logging sensitive information unnecessarily.

Structured logs also make centralized search and automated analysis easier.

---

## 4. Distributed Tracing

A single commerce request may cross many services.

A trace can connect those operations:

```text
Request
 ├── Commerce API
 │    ├── Product Service
 │    └── Pricing Service
 │
 ├── Inventory Service
 │
 └── Payment Provider
```

This makes latency and failure propagation easier to understand.

---

## 5. Correlation IDs

A correlation identifier can connect related operations across systems.

```text
Customer Request
      ↓
Correlation ID
      ↓
API
      ↓
Integration
      ↓
External System
```

When an incident occurs, engineers can follow the transaction across boundaries.

---

## 6. Business Observability

Technical monitoring alone is insufficient.

Commerce platforms should also expose business signals.

Examples:

- Orders created
- Checkout failures
- Payment failures
- Inventory allocation failures
- Return failures
- Promotion application failures

A system can be technically healthy while a critical business process is broken.

---

## 7. Alerting

Alerts should correspond to actionable conditions.

Poor:

> CPU is high.

Better:

> Checkout error rate exceeded the defined threshold for the required observation window.

Alerting should minimize noise and clearly identify ownership.

---

## 8. Integration Observability

External integrations should expose:

- Request counts
- Success/failure rates
- Latency
- Timeout rates
- Retry counts
- Dead-letter messages
- Synchronization delays

This is particularly important when the external system is outside the commerce team's control.

---

## 9. AI-Assisted Observability

Observability data can potentially support intelligent diagnostics.

A conceptual flow:

```text
Telemetry
   ↓
Correlation
   ↓
Pattern Detection
   ↓
Possible Cause
   ↓
Engineer Investigation
```

AI-generated explanations should remain distinguishable from confirmed system facts.

---

## 10. Common Failure Modes

### Logs without correlation

**Problem:** Individual services log independently.

**Impact:** Difficult incident reconstruction.

### Metrics without business context

**Problem:** Infrastructure looks healthy while checkout fails.

**Impact:** Delayed detection.

### Excessive logging

**Problem:** Everything is logged.

**Impact:** Cost, noise, and potential sensitive-data exposure.

### Alerts without ownership

**Problem:** Teams receive alerts they cannot act on.

**Impact:** Alert fatigue.

---

## 11. Architectural Principle

> **Observability should make both system behavior and business impact understandable.**