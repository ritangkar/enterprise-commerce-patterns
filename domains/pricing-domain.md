# Pricing Domain

Pricing determines the commercial value presented for a product within a specific business context.

Enterprise pricing can become complex because the same product may have different prices based on:

* Customer
* Organization
* Market
* Currency
* Quantity
* Contract
* Channel
* Date
* Sales agreement

Pricing should therefore be treated as a dedicated business capability rather than a property embedded directly into the product model.

---

## 1. Domain Responsibilities

The pricing domain typically manages:

* Base prices
* Price lists
* Customer-specific prices
* Contract pricing
* Quantity breaks
* Currency
* Effective dates
* Market-specific pricing
* Price precedence
* Price calculation

It should remain separate from promotional discounts where the business model requires that distinction.

---

## 2. Product vs Price

A product answers:

> What is being sold?

Pricing answers:

> At what commercial value should this product be offered in this context?

Therefore:

```text
Product
   │
   └── Product Identity

Pricing
   │
   ├── Price
   ├── Currency
   ├── Customer Context
   ├── Quantity
   └── Validity
```

This separation allows the same product to participate in multiple pricing models.

---

## 3. Pricing Context

A price calculation may depend on several dimensions.

Conceptually:

```text
Product
+
Customer Context
+
Market
+
Currency
+
Quantity
+
Date
+
Channel
        ↓
   Price Resolution
        ↓
Applicable Price
```

Not every implementation needs every dimension.

The important principle is to define the dimensions explicitly.

---

## 4. Price Precedence

When multiple prices are applicable, the platform needs deterministic resolution.

For example:

```text
Contract Price
      ↓
Customer-Specific Price
      ↓
Segment Price
      ↓
Channel Price
      ↓
Market Price
      ↓
Base Price
```

The actual precedence is business-specific.

What matters architecturally is that the rule is:

* Explicit
* Deterministic
* Testable
* Observable

Avoid allowing precedence to emerge accidentally from database ordering or implementation side effects.

---

## 5. Quantity-Based Pricing

B2B commerce frequently uses volume pricing.

Example:

```text
1–9 units      → Price A
10–49 units    → Price B
50–99 units    → Price C
100+ units     → Price D
```

Quantity pricing should be calculated consistently across:

* Product detail
* Cart
* Checkout
* Order
* APIs

The final order should preserve enough pricing information to explain how the commercial value was determined.

---

## 6. Currency and Market

Currency conversion should not be treated as a simple formatting operation.

Important considerations include:

* Currency precision
* Rounding
* Exchange-rate ownership
* Effective dates
* Market-specific pricing
* Tax treatment

The commerce platform should define where conversion responsibility resides.

---

## 7. Pricing vs Promotion

A useful distinction is:

**Pricing**

> Determines the applicable commercial price.

**Promotion**

> Applies a business incentive or adjustment to that price.

Conceptually:

```text
Product
   ↓
Base / Applicable Price
   ↓
Promotion Evaluation
   ↓
Adjusted Commercial Value
```

The exact calculation model varies by business.

---

## 8. Price Snapshots

Once an order is placed, the system should preserve the commercial values used for that transaction.

This prevents historical orders from changing when future price lists change.

Conceptually:

```text
Current Pricing
      ↓
Order Placement
      ↓
Transaction Price Snapshot
```

This is particularly important for:

* Auditing
* Returns
* Invoicing
* Customer service
* Dispute resolution

---

## 9. Performance Considerations

Pricing is often a high-frequency operation.

Poor pricing architecture can create latency across:

* Product listing
* Product detail
* Cart
* Checkout
* Reorder flows
* APIs

Consider:

* Efficient lookup structures
* Appropriate caching
* Batch evaluation
* Context-aware invalidation
* Avoiding repeated calculations

Performance optimization should not compromise pricing correctness.

---

## 10. Common Failure Modes

### Pricing embedded into product entities

**Problem:** Product records become overloaded with customer and market-specific prices.

**Impact:** Poor scalability and difficult maintenance.

### Hidden precedence rules

**Problem:** Different services resolve competing prices differently.

**Impact:** Inconsistent customer experiences.

### No transaction snapshot

**Problem:** Historical orders depend on current pricing.

**Impact:** Incorrect order history and reconciliation issues.

### Excessive synchronous dependencies

**Problem:** Every price request requires multiple remote systems.

**Impact:** Increased latency and availability risk.

---

## 11. Architectural Principle

> **Pricing should be deterministic, contextual, explainable, and independently evolvable from the product domain.**
