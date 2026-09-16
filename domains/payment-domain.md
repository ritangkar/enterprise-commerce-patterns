# Payment Domain

Payment is a high-sensitivity commerce capability responsible for converting customer intent into a financially authorized transaction.

It typically interacts with:

- Payment service providers
- Banks
- Fraud systems
- Order systems
- Refund systems
- Accounting platforms

Payment architecture should prioritize **security, correctness, idempotency, and reconciliation**.

---

## 1. Domain Responsibilities

Depending on the architecture, payment capabilities may include:

- Payment method selection
- Payment authorization
- Capture
- Void
- Refund
- Payment status
- Transaction references
- Reconciliation
- Payment event handling

Sensitive payment credentials should remain within appropriately controlled payment infrastructure.

---

## 2. Authorization vs Capture

These are distinct financial operations.

```text
Payment
   ↓
Authorization
   ↓
Capture
   ↓
Settlement
```

Some commerce models capture immediately.

Others may authorize first and capture later.

The lifecycle should reflect the actual payment contract.

---

## 3. Payment State

Payment status should not be inferred solely from order status.

Conceptually:

```text
Order
 ├── Commercial State
 │
 └── Payment State
      ├── Pending
      ├── Authorized
      ├── Captured
      ├── Failed
      ├── Voided
      └── Refunded
```

This separation improves reconciliation.

---

## 4. Idempotency

Payment operations must be safe against retries.

For example:

```text
Payment Request
      ↓
Idempotency Key
      ↓
Payment Provider
      ↓
Stable Transaction Result
```

Without idempotency, a network retry could potentially produce multiple financial operations.

---

## 5. Asynchronous Payment Events

Payment providers may notify commerce asynchronously.

Example:

```text
Payment Provider
      ↓
Payment Event
      ↓
Commerce Payment State
      ↓
Order / Fulfillment Processing
```

Consumers should validate event authenticity and handle duplicate delivery safely.

---

## 6. Failure Handling

Payment failures may include:

- Customer decline
- Provider timeout
- Network failure
- Fraud rejection
- Duplicate request
- Provider-side processing delay

Not every timeout means the payment failed.

For example:

```text
Request
  ↓
Timeout
  ↓
Unknown State
  ↓
Status Verification
  ↓
Confirmed Result
```

Treating every timeout as failure can create duplicate transactions.

---

## 7. Refunds

Refunds may occur because of:

- Full cancellation
- Partial cancellation
- Product return
- Fulfillment failure
- Customer-service adjustment

A refund should reference the original financial transaction wherever possible.

---

## 8. Reconciliation

Payment systems require reconciliation between internal records and external providers.

```text
Commerce Payment Records
          +
Provider Transaction Records
          ↓
       Reconcile
          ↓
     Difference
          ↓
 Investigation / Correction
```

Reconciliation should be designed as a normal operational capability.

---

## 9. Security

Payment architecture should consider:

- Tokenization
- Encryption
- Least-privilege access
- Sensitive-data minimization
- Secure communication
- Auditability
- Fraud controls
- Regulatory requirements

The commerce application should avoid handling sensitive payment information unnecessarily.

---

## 10. Common Failure Modes

### Payment and order are tightly coupled

**Problem:** Both must succeed inside one synchronous transaction.

**Impact:** Fragility across distributed systems.

### Timeout treated as failure

**Problem:** Unknown payment state is interpreted as declined.

**Impact:** Duplicate attempts or incorrect customer messaging.

### No reconciliation

**Problem:** Financial differences remain undetected.

**Impact:** Accounting and customer-service issues.

### Payment credentials stored unnecessarily

**Problem:** Sensitive data enters commerce systems without a business need.

**Impact:** Increased security and compliance exposure.

---

## 11. Architectural Principle

> **Payment architecture should assume retries, partial failures, asynchronous outcomes, and reconciliation from the beginning.**