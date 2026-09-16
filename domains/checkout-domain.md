# Checkout Domain

Checkout converts a customer's cart intent into a validated transaction that can become an order.

It is one of the most sensitive commerce flows because it combines:

* Customer identity
* Cart state
* Pricing
* Promotions
* Inventory
* Delivery
* Tax
* Payment
* Order creation

Checkout should therefore orchestrate these capabilities without becoming the permanent owner of their underlying business logic.

---

## 1. Domain Responsibilities

Checkout typically coordinates:

* Cart validation
* Customer validation
* Delivery selection
* Address validation
* Shipping calculation
* Tax calculation
* Payment authorization
* Final pricing validation
* Promotion validation
* Order creation

It should act primarily as an orchestration boundary.

---

## 2. Checkout Flow

A conceptual flow:

```text
Cart
 ↓
Validate Customer
 ↓
Validate Products
 ↓
Validate Pricing
 ↓
Validate Promotions
 ↓
Validate Inventory
 ↓
Select Delivery
 ↓
Calculate Taxes
 ↓
Authorize Payment
 ↓
Create Order
```

The exact sequence varies depending on the commerce model.

---

## 3. Revalidation

Information can change between cart creation and checkout.

Examples:

* Price changed
* Promotion expired
* Product became unavailable
* Delivery option changed
* Tax changed
* Customer permissions changed

Therefore checkout should perform appropriate final validation.

```text
Cart State
   ↓
Checkout Validation
   ↓
Current Commercial State
   ↓
Proceed / Correct
```

---

## 4. Payment Authorization

Payment authorization should remain distinct from order creation.

Conceptually:

```text
Checkout
   ↓
Payment Authorization
   ↓
Order Creation
```

The architecture must also define failure handling.

For example:

```text
Payment Authorized
      ↓
Order Creation Fails
      ↓
Compensation / Recovery
```

This is a distributed transaction problem and should not rely on a single database transaction spanning external systems.

---

## 5. Inventory Validation

Inventory may be:

* Checked during checkout
* Reserved during checkout
* Reserved earlier
* Confirmed by fulfillment systems

The architecture should explicitly define the chosen model.

Avoid representing a temporary availability check as a guaranteed reservation.

---

## 6. Delivery and Fulfillment Context

Delivery options may depend on:

* Customer address
* Product characteristics
* Inventory location
* Carrier availability
* Service level
* Market
* Order value

Checkout should request delivery options from the appropriate capability rather than embedding fulfillment rules directly.

---

## 7. Tax

Tax calculation can depend on:

* Customer location
* Ship-to address
* Product classification
* Market
* Transaction type

Tax responsibility should be clearly assigned.

Where an external tax service is used, checkout should handle the integration boundary and failure behavior without becoming the tax calculation engine itself.

---

## 8. Idempotent Order Creation

Order creation must be protected against duplicate requests.

For example:

```text
Checkout Request
      ↓
Idempotency Key
      ↓
Order Creation
      ↓
Stable Result
```

This is particularly important when:

* Network requests time out
* Clients retry
* Payment providers retry callbacks
* Distributed services experience transient failures

The customer should not receive two orders because the original response was lost.

---

## 9. Checkout State

A checkout process may move through explicit states:

```text
Started
  ↓
Validated
  ↓
Delivery Selected
  ↓
Payment Authorized
  ↓
Order Created
  ↓
Completed
```

Failure states should also be observable.

Avoid relying exclusively on implicit database state to determine where a checkout process stopped.

---

## 10. Security

Checkout should receive heightened security attention.

Important controls include:

* Authentication
* Authorization
* Secure payment handling
* Input validation
* Sensitive-data protection
* Rate limiting
* Auditability
* Fraud controls where applicable

Payment credentials should not be unnecessarily stored or exposed within commerce services.

---

## 11. Common Failure Modes

### Checkout becomes a monolith

**Problem:** All pricing, inventory, tax, payment, and fulfillment logic is implemented inside checkout.

**Impact:** Difficult testing and high coupling.

### No final validation

**Problem:** Cart state is assumed to remain valid.

**Impact:** Incorrect orders.

### Duplicate order creation

**Problem:** Retry behavior is not idempotent.

**Impact:** Duplicate transactions.

### Distributed failure is ignored

**Problem:** Payment succeeds but order creation fails, with no recovery strategy.

**Impact:** Reconciliation and customer-service issues.

---

## 12. Architectural Principle

> **Checkout should orchestrate the path from customer intent to transaction while preserving clear ownership of each business capability.**

A well-designed checkout flow is therefore not necessarily the one with the fewest services—it is the one with clear responsibilities, predictable failure behavior, and a consistent customer outcome.
