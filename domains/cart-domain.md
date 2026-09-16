# Cart Domain

The cart represents the customer's current commercial intent before an order is created.

It is a highly dynamic domain and often sits at the intersection of:

* Product
* Pricing
* Promotion
* Inventory
* Customer
* Delivery
* Checkout

Because of this, cart architecture should minimize unnecessary coupling while maintaining a consistent commercial state.

---

## 1. Domain Responsibilities

The cart domain typically manages:

* Cart identity
* Cart ownership
* Cart entries
* Quantities
* Selected products
* Applied promotions
* Price calculations
* Cart totals
* Cart lifecycle
* Guest-to-customer transition
* Cart merging

It should not become the permanent owner of order fulfillment or payment state.

---

## 2. Cart Model

A simplified structure:

```text
Cart
├── Customer Context
├── Entries
│   ├── Product
│   ├── Quantity
│   └── Commercial Values
├── Promotions
├── Delivery Context
└── Totals
```

The actual model may be more complex, but the domain boundary should remain clear.

---

## 3. Cart as a Stateful Object

Unlike a simple product query, a cart represents changing customer intent.

Typical lifecycle:

```text
Created
  ↓
Modified
  ↓
Validated
  ↓
Checkout Started
  ↓
Order Created
  ↓
Closed
```

Carts may also expire or be abandoned.

---

## 4. Adding an Item

A simplified flow:

```text
Add Product
     ↓
Validate Product
     ↓
Validate Customer Eligibility
     ↓
Determine Quantity Rules
     ↓
Determine Applicable Price
     ↓
Evaluate Relevant Promotions
     ↓
Update Cart
```

Not every validation needs to be synchronous with every add-to-cart operation.

The architecture should distinguish fast local validations from expensive downstream checks.

---

## 5. Cart Totals

Cart totals should be derived consistently.

Conceptually:

```text
Line Values
   +
Shipping
   +
Taxes
   -
Discounts
   ↓
Final Cart Value
```

The exact financial model varies by jurisdiction and business.

The key principle is that calculations should be deterministic and traceable.

---

## 6. Inventory and Cart

Adding an item to a cart does not necessarily mean inventory has been reserved.

These are different concepts:

```text
Cart Intent
     ≠
Inventory Reservation
```

A business may choose to reserve inventory during cart creation, checkout, or another controlled stage.

The decision depends on:

* Product scarcity
* Reservation cost
* Order volume
* Fulfillment model
* Customer experience requirements

---

## 7. Cart Concurrency

The same cart may be accessed from:

* Multiple browser tabs
* Mobile and web channels
* Assisted commerce
* APIs
* Background processes

Concurrency controls should prevent lost updates and inconsistent totals.

Potential approaches include:

* Version checks
* Optimistic concurrency
* Controlled locking
* Atomic updates

The appropriate mechanism depends on workload and architecture.

---

## 8. Guest-to-Customer Cart Merge

When a guest authenticates, two carts may exist:

```text
Guest Cart
    +
Customer Cart
    ↓
Merge Strategy
    ↓
Customer Cart
```

Merge rules should explicitly define:

* Duplicate products
* Quantities
* Promotions
* Invalid products
* Price changes
* Inventory changes

Silent or inconsistent merging creates difficult customer-facing problems.

---

## 9. Cart Expiration

Carts may become stale.

Expiration policies should consider:

* Business requirements
* Inventory behavior
* Pricing volatility
* Storage cost
* Customer expectations

An expired cart should not automatically imply that the underlying customer order has expired or been cancelled.

---

## 10. Common Failure Modes

### Cart becomes a mini order system

**Problem:** Fulfillment, payment, shipment, and service logic are embedded into cart processing.

**Impact:** Excessive coupling.

### Cart assumes inventory availability forever

**Problem:** Availability is treated as guaranteed after add-to-cart.

**Impact:** Checkout failures.

### Cart totals are recalculated inconsistently

**Problem:** Different channels calculate totals differently.

**Impact:** Price discrepancies.

### Cart merge is undefined

**Problem:** Guest and authenticated carts have no deterministic merge behavior.

**Impact:** Lost or duplicated customer intent.

---

## 11. Architectural Principle

> **A cart represents intent, not a completed commercial transaction.**

The architecture should allow that intent to evolve without prematurely binding it to order or fulfillment processes.
