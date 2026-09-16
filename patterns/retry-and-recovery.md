# Retry and Recovery

Failures are normal in distributed systems.

A downstream system may be temporarily unavailable, a network request may time out, or a message may fail processing.

A resilient commerce architecture distinguishes between **temporary failures that may recover automatically** and **permanent failures that require correction or intervention.**

---

# 1. Failure Classification

A useful first step is classification.

```text
Failure
  │
  ├── Temporary?
  │      │
  │      └── Retry may help
  │
  └── Permanent?
         │
         └── Retry may not help
```

Examples of potentially temporary failures:

* timeout
* temporary service unavailability
* transient network issue
* rate limiting

Examples of potentially permanent failures:

* invalid data
* failed business validation
* unsupported request
* authorization failure

---

# 2. Basic Retry Flow

```text
Request
  │
  ▼
Dependency
  │
  ├── Success → Complete
  │
  └── Temporary Failure
          │
          ▼
        Retry
          │
          ├── Success → Complete
          │
          └── Failure → Recovery
```

---

# 3. Backoff

Immediate repeated retries can increase load on an already struggling dependency.

Instead, retry intervals can increase over time.

Conceptually:

```text
Attempt 1 → immediate
Attempt 2 → short delay
Attempt 3 → longer delay
Attempt 4 → longer delay
```

This is commonly referred to as backoff.

Jitter can also be introduced to reduce synchronized retry behavior across many clients.

---

# 4. Retry Limits

Retries should have boundaries.

Without limits:

```text
Failure
 ↓
Retry
 ↓
Failure
 ↓
Retry
 ↓
Failure
 ↓
...
```

This can create unnecessary load and delay operational recovery.

A resilient design defines:

* maximum attempts
* maximum elapsed time
* retryable errors
* non-retryable errors
* fallback behavior

---

# 5. Dead-Letter Handling

When asynchronous processing repeatedly fails, the message may need to be moved to an operational queue.

```text
Message
   │
   ▼
Consumer
   │
   ├── Success → Complete
   │
   └── Repeated Failure
             │
             ▼
        Dead-Letter / Error Store
             │
             ▼
        Investigation / Replay
```

This prevents one problematic message from blocking the entire processing pipeline.

---

# 6. Recovery

Recovery should answer:

> What happens after the system becomes healthy again?

Possible mechanisms include:

* replay
* reconciliation
* manual correction
* compensating action
* reprocessing
* state repair

Recovery should be observable rather than dependent on hidden manual actions.

---

# 7. Retry vs Compensation

A retry attempts the **same operation again**.

A compensating action attempts to **correct an already completed business effect**.

Example:

```text
Operation A succeeds
Operation B fails
```

Retrying B may be appropriate if B failed transiently.

But if A created an external side effect that cannot be rolled back, the architecture may need a compensating process.

---

# 8. Avoid Retrying Everything

A useful rule:

> Retry failures that are plausibly temporary and safe to repeat.

Do not blindly retry:

* validation errors
* authorization failures
* malformed requests
* business-rule failures

Retries should be based on error semantics.

---

# 9. Observability

A retry mechanism without observability can hide systemic problems.

Monitor:

* retry count
* retry reason
* retry latency
* final failure rate
* dead-letter volume
* recovery duration
* affected business transactions

A high retry rate may be a symptom of a deeper architectural problem.

---

# 10. Recovery Checklist

```text
[ ] Which failures are retryable?
[ ] Which failures are permanent?
[ ] Is the operation idempotent?
[ ] What is the retry limit?
[ ] Is backoff required?
[ ] Is jitter useful?
[ ] What happens after retries are exhausted?
[ ] Can failed messages be inspected?
[ ] Can they be replayed safely?
[ ] Is reconciliation available?
[ ] Are failures observable?
```

---

# Final Principle

> **Retry is a recovery mechanism, not a substitute for understanding failure.**

A mature architecture knows why an operation failed, whether retry is safe, when to stop retrying, and how the business state can ultimately be recovered.
