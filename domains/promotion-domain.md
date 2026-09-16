# Promotion Domain

The promotion domain represents commercial incentives applied to eligible commerce transactions.

Promotions can influence:

* Products
* Carts
* Orders
* Customers
* Quantities
* Channels
* Dates
* Categories
* Customer segments

Because promotion logic can grow rapidly, it requires strong separation between **eligibility**, **calculation**, and **application**.

---

## 1. Domain Responsibilities

The promotion domain typically manages:

* Promotion definitions
* Eligibility rules
* Conditions
* Benefits
* Priority
* Validity
* Usage constraints
* Coupon or code references
* Promotion stacking rules

It should not own the core product, customer, or order domains.

---

## 2. Promotion Structure

A useful conceptual model is:

```text
Promotion
├── Eligibility
├── Conditions
├── Benefit
├── Validity
├── Usage Limits
└── Priority
```

For example:

```text
Eligibility
     ↓
Does customer qualify?
     ↓
Do products qualify?
     ↓
Does cart qualify?
     ↓
Benefit Calculation
     ↓
Apply Adjustment
```

---

## 3. Types of Promotions

Common patterns include:

### Product discount

```text
Product → 10% discount
```

### Category discount

```text
Category → 15% discount
```

### Cart threshold

```text
Cart ≥ threshold
       ↓
Fixed discount
```

### Quantity incentive

```text
Buy N
 ↓
Receive benefit
```

### Buy X, get Y

```text
Purchase X
    ↓
Receive Y benefit
```

### Customer-specific promotion

```text
Eligible Customer
       ↓
Promotion
```

These patterns can be combined, but complexity should be controlled.

---

## 4. Eligibility vs Benefit

One of the most important design separations is:

```text
Eligibility
    ↓
Can this promotion apply?

Benefit
    ↓
What value does it provide?
```

This makes promotion rules easier to reason about and test.

---

## 5. Promotion Stacking

Multiple promotions may apply to the same cart.

The system should define:

* Whether stacking is allowed
* Which promotions take precedence
* Whether discounts are cumulative
* Whether one promotion blocks another
* Whether order of evaluation matters

Example:

```text
Promotion A
     ↓
Promotion B
     ↓
Final Adjustment
```

The evaluation strategy must be deterministic.

---

## 6. Coupon Codes

Coupon-based promotions introduce another dimension.

A coupon may have:

* Global usage limits
* Per-customer limits
* Validity periods
* Minimum cart value
* Product restrictions
* Channel restrictions

Coupon validation should be performed close to the point where the promotion is actually applied.

---

## 7. Promotion Lifecycle

Promotions typically follow:

```text
Draft
 ↓
Approved
 ↓
Scheduled
 ↓
Active
 ↓
Expired
```

Lifecycle management should prevent accidental activation or modification of live commercial rules.

---

## 8. Performance

Promotion evaluation can become computationally expensive.

Consider:

* Rule pre-filtering
* Candidate reduction
* Efficient condition evaluation
* Caching where safe
* Avoiding unnecessary repeated calculations
* Limiting promotion complexity

A promotion engine should not become a bottleneck for every product or cart operation.

---

## 9. Auditability

A customer or service representative should be able to understand:

```text
Promotion
   ↓
Eligibility
   ↓
Benefit
   ↓
Applied Adjustment
```

This is especially important when customers ask:

> Why did I receive this discount?

Explainability is therefore a commerce requirement, not only an AI concern.

---

## 10. Common Failure Modes

### Promotions become arbitrary business code

**Problem:** Every campaign introduces custom implementation.

**Impact:** Increasing maintenance and regression risk.

### Rules are not deterministic

**Problem:** Evaluation depends on execution order or hidden state.

**Impact:** Inconsistent results.

### Promotion logic leaks into checkout

**Problem:** Checkout owns promotion rules directly.

**Impact:** Difficult reuse and testing.

### No historical explanation

**Problem:** Orders only store the final amount.

**Impact:** Difficult reconciliation and customer support.

---

## 11. Architectural Principle

> **Promotion engines should evaluate explicit business rules rather than becoming a collection of campaign-specific customizations.**
