# Resilience

Enterprise commerce systems operate across distributed dependencies.

Failures are therefore normal operating conditions rather than theoretical exceptions.

A resilient architecture should:

- Detect failure
- Contain failure
- Recover where possible
- Preserve important transactions
- Provide useful customer behavior
- Make failures observable

---

## 1. Failure Domains

A commerce request can fail at multiple layers:

```text
Frontend
   ↓
Commerce
   ↓
Database
   ↓
Integration
   ↓
External System
```

Each dependency introduces a potential failure domain.

---

## 2. Failure Classification

Failures should be classified before choosing recovery behavior.

Examples:

### Transient

Likely to recover.

Examples:

- Temporary network failure
- Rate limit
- Short service interruption

### Permanent

Retrying will not help.

Examples:

- Invalid request
- Authorization failure
- Unsupported operation

### Unknown

The system cannot determine whether the operation succeeded.

Examples:

- Payment timeout
- Connection lost after request submission

Unknown outcomes require status verification or reconciliation rather than blind retries.

---

## 3. Retry Strategy

A retry policy should define:

- Which failures are retryable
- Maximum attempts
- Backoff
- Jitter
- Timeout
- Overall deadline

Conceptually:

```text
Failure
  ↓
Retryable?
  ├── No → Fail / Compensate
  │
  └── Yes
       ↓
    Backoff
       ↓
      Retry
```

Retries should not amplify an existing outage.

---

## 4. Circuit Breaking

Repeated calls to an unhealthy dependency can worsen an incident.

A circuit breaker can move through:

```text
Closed
  ↓
Failure Threshold
  ↓
Open
  ↓
Recovery Check
  ↓
Half-Open
  ↓
Closed
```

This protects both the calling service and the failing dependency.

---

## 5. Timeouts

Every external dependency should have an intentional timeout.

Without timeouts:

```text
Slow Dependency
      ↓
Waiting Requests
      ↓
Thread / Connection Exhaustion
      ↓
System Degradation
```

Timeouts should reflect actual business requirements rather than arbitrary defaults.

---

## 6. Graceful Degradation

Not every capability is equally critical.

For example:

```text
Checkout
 ├── Payment → Critical
 ├── Inventory → Critical
 ├── Recommendation → Optional
 └── Analytics → Optional
```

If recommendations fail, checkout may continue.

If payment fails, checkout should not.

This distinction enables graceful degradation.

---

## 7. Bulkheads

Independent resource pools can prevent one workload from consuming everything.

Conceptually:

```text
Commerce Platform
 ├── Checkout Resources
 ├── Search Resources
 ├── Background Processing
 └── Integration Workers
```

A failure in one area should not automatically exhaust resources in another.

---

## 8. Recovery and Compensation

Distributed transactions may require compensating actions.

Example:

```text
Payment Authorized
      ↓
Order Creation Failed
      ↓
Compensation / Reconciliation
      ↓
Payment Reversal or Recovery
```

Compensation is different from database rollback.

Once an external system has committed a transaction, the original operation cannot necessarily be rolled back atomically.

---

## 9. Dead-Letter Handling

Messages that repeatedly fail should not block the entire pipeline.

```text
Message
  ↓
Retry
  ↓
Retry
  ↓
Dead-Letter
  ↓
Investigation / Replay
```

Dead-letter queues should be observable and operationally actionable.

---

## 10. Common Failure Modes

### Retry everything

**Problem:** Permanent failures are repeatedly retried.

**Impact:** Waste and downstream pressure.

### No timeout

**Problem:** Requests wait indefinitely.

**Impact:** Resource exhaustion.

### Cascading failure

**Problem:** One dependency failure propagates through synchronous calls.

**Impact:** Broad platform outage.

### No recovery path

**Problem:** Failed distributed transactions remain unresolved.

**Impact:** Manual reconciliation and inconsistent state.

---

## 11. Architectural Principle

> **Resilience is not preventing every failure; it is designing the system so that failures remain bounded, observable, and recoverable.**