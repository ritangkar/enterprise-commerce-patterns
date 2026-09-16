# Returns Domain

Returns represent the reverse side of the commerce lifecycle.

A complete commerce architecture should treat returns as a first-class business capability rather than an afterthought to order processing.

Returns may involve:

- Customer requests
- Eligibility
- Return authorization
- Logistics
- Inspection
- Refunds
- Exchanges
- Inventory disposition

---

## 1. Domain Responsibilities

The returns domain may manage:

- Return requests
- Return eligibility
- Return reasons
- Return authorization
- Return lines
- Return shipments
- Inspection outcomes
- Refund requests
- Exchange requests
- Return status

---

## 2. Return Lifecycle

A conceptual lifecycle:

```text
Return Requested
       ↓
Eligibility Checked
       ↓
Return Authorized
       ↓
Item Received
       ↓
Inspection
       ↓
Disposition
       ↓
Refund / Exchange
       ↓
Completed
```

Not every return requires every stage.

---

## 3. Return Eligibility

Eligibility may depend on:

- Order age
- Product category
- Customer policy
- Product condition
- Purchase channel
- Return reason
- Market
- Previous return history

Eligibility rules should be explicit and testable.

---

## 4. Partial Returns

Customers may return only part of an order.

```text
Order
├── Item A → Returned
├── Item B → Kept
└── Item C → Returned
```

The system must preserve the relationship between:

```text
Original Order
      ↓
Returned Line
      ↓
Refund / Exchange
```

---

## 5. Refund Relationship

A return does not necessarily equal an immediate refund.

A refund may depend on:

- Receipt of the product
- Inspection
- Return policy
- Payment state
- Item condition

The architecture should therefore separate:

```text
Return Status
      ≠
Refund Status
```

---

## 6. Inventory Disposition

Returned products may have different outcomes:

```text
Returned Item
     ↓
Inspection
     ├── Restock
     ├── Refurbish
     ├── Dispose
     └── Return to Supplier
```

This decision may belong to operational systems rather than commerce itself.

---

## 7. Exchanges

An exchange can be modeled as:

```text
Original Item
     ↓
Return
     +
Replacement Item
```

Pricing, inventory, and payment differences may need separate handling.

---

## 8. Common Failure Modes

### Return logic embedded into order logic

**Problem:** Orders become responsible for every reverse-transaction scenario.

**Impact:** Increasing complexity.

### Refund assumed immediately

**Problem:** Return authorization automatically triggers refund.

**Impact:** Financial inconsistency.

### Partial returns unsupported

**Problem:** System assumes the whole order is returned.

**Impact:** Incorrect refunds and order state.

### No return traceability

**Problem:** Return cannot be reliably linked to original transaction.

**Impact:** Customer-service and reconciliation problems.

---

## 9. Architectural Principle

> **A return is a business transaction related to an order—not simply a reversal of the order itself.**