# Idempotency in Commerce Systems

Distributed commerce systems frequently retry operations.

Networks fail.

Responses time out.

Messages can be delivered more than once.

An operation is **idempotent** when repeating the same operation does not create an unintended additional business effect.

---

# 1. Why It Matters

Consider:

```text
Client
  │
  │ Create Order
  ▼
Commerce
  │
  ▼
External System
  │
  X── Timeout
```

The caller does not know whether the external system completed the operation.

A retry could produce:

```text
First request  → Order created
Second request → Another order created
```

This is a serious business problem.

---

# 2. Idempotency Key

A common approach is to associate a unique key with the business operation.

```text
Request
   │
   ▼
Idempotency Key
   │
   ▼
Processing
```

On retry:

```text
Same Idempotency Key
        │
        ▼
Previously processed?
     /        \
   Yes         No
   │            │
Return       Process
existing
result
```

---

# 3. Where Idempotency Matters

Particularly important areas include:

* order creation
* payment operations
* inventory reservations
* shipment creation
* external synchronization
* message consumers

Not every read operation requires the same treatment.

---

# 4. Consumer Idempotency

Event consumers may receive the same event more than once.

For example:

```text
OrderConfirmed
      │
      ├── Delivery #1
      │
      └── Delivery #2
```

The consumer should avoid producing two unintended side effects.

A conceptual approach:

```text
Event ID
   │
   ▼
Processed?
 ├── Yes → Ignore / return prior result
 └── No → Process + record
```

---

# 5. Idempotency vs Deduplication

These concepts are related but not identical.

### Deduplication

Detects that the same event/request has appeared before.

### Idempotency

Ensures repeated processing does not produce an unintended business outcome.

A system may use deduplication as one mechanism for achieving idempotent behavior.

---

# 6. Business Semantics

Technical uniqueness is not enough.

For example:

```text
Request ID = ABC123
```

does not automatically guarantee business correctness.

The system must define:

> What does it mean for this operation to have already happened?

The answer should be tied to the business transaction.

---

# 7. Idempotency and Retries

Retries and idempotency should be designed together.

```text
Operation
   │
   ▼
Failure
   │
   ▼
Retry
   │
   ▼
Same logical operation
```

Without idempotency, retries can amplify failures.

With idempotency, retries can become a controlled recovery mechanism.

---

# 8. Idempotency Checklist

```text
[ ] What operation can be retried?
[ ] What uniquely identifies the business operation?
[ ] Can duplicate requests occur?
[ ] Can duplicate messages occur?
[ ] What state indicates successful completion?
[ ] What should happen on retry?
[ ] How long should idempotency state be retained?
[ ] How are conflicting requests handled?
```

---

# Final Principle

> **If an operation can be retried, design its business semantics so that retrying it is safe.**

Idempotency is not merely a messaging concern.

It is a fundamental reliability pattern for distributed commerce.
