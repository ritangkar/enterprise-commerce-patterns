# Order Domain

The order domain represents the durable commercial transaction created after checkout.

An order is fundamentally different from a cart.

A cart represents **current customer intent**.

An order represents a **committed business transaction**.

Once created, an order should become increasingly stable and auditable.

---

## 1. Domain Responsibilities

The order domain typically manages:

* Order identity
* Order lines
* Customer context
* Transaction values
* Order status
* Payment references
* Delivery references
* Fulfillment references
* Order history
* Cancellation state
* Commercial snapshots

It should not become the owner of:

* Live inventory
* Payment credentials
* Carrier operations
* Customer authentication

Those belong to their respective capabilities.

---

## 2. Order Creation

A simplified flow is:

```text
Checkout
   ↓
Validated Transaction
   ↓
Order Creation
   ↓
Order Persisted
   ↓
Order Event
   ↓
Downstream Processing
```

Once the order is successfully persisted, downstream processes can react asynchronously where appropriate.

---

## 3. Order Snapshot

An order should preserve the commercial state that existed when the transaction was committed.

This can include:

* Product information required for the transaction
* Quantity
* Unit price
* Discounts
* Taxes
* Shipping charges
* Currency
* Customer context
* Delivery information

The purpose is historical accuracy.

Future changes to the catalog or pricing model should not rewrite historical transactions.

---

## 4. Order Lifecycle

A generic lifecycle might look like:

```text
Created
   ↓
Confirmed
   ↓
Processing
   ↓
Fulfillment
   ↓
Completed
```

Alternative paths may include:

```text
Created
   ↓
Cancelled
```

or:

```text
Processing
   ↓
Partially Fulfilled
   ↓
Completed
```

The exact state model should reflect actual business processes rather than implementation convenience.

---

## 5. Order Status vs Fulfillment Status

These concepts should not automatically be treated as the same thing.

For example:

```text
Order
 ├── Commercial Status
 │
 └── Fulfillment Status
```

An order may be commercially confirmed while fulfillment is still pending.

Likewise, a partially fulfilled order may remain commercially open.

Separating these concepts reduces ambiguity.

---

## 6. Order Events

Order lifecycle changes can generate business events.

Examples:

```text
OrderCreated
OrderConfirmed
OrderCancelled
OrderPartiallyFulfilled
OrderFulfilled
OrderCompleted
```

Consumers may include:

* Fulfillment
* Notifications
* Customer service
* Analytics
* ERP integrations
* Loyalty systems

Consumers should not depend on direct synchronous coupling unless required.

---

## 7. Order Modification

After order creation, modifications should be controlled.

Possible changes include:

* Address updates
* Quantity adjustments
* Cancellation
* Delivery changes

Each change should consider the downstream state.

For example:

```text
Order Created
      ↓
Payment
      ↓
Fulfillment Started
```

A modification allowed immediately after creation may no longer be possible once fulfillment has begun.

---

## 8. Order History

Customers and support teams often need a consistent transaction history.

The system should distinguish between:

* Current state
* Historical events
* Financial values
* Fulfillment events

This improves traceability and customer-service capabilities.

---

## 9. Common Failure Modes

### Order becomes an integration hub

**Problem:** Every external process is directly embedded into order logic.

**Impact:** Tight coupling and difficult evolution.

### Historical values are recalculated

**Problem:** Current product/pricing data is used to reconstruct old orders.

**Impact:** Incorrect historical information.

### Status becomes overloaded

**Problem:** One status field represents payment, fulfillment, cancellation, and commercial state.

**Impact:** Ambiguous workflows.

### No durable event history

**Problem:** Important lifecycle transitions cannot be reconstructed.

**Impact:** Difficult support and reconciliation.

---

## 10. Architectural Principle

> **An order is a durable business record, not merely the final state of a cart.**

The order domain should prioritize correctness, traceability, stability, and clear downstream integration boundaries.
