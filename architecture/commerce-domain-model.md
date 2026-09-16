# Commerce Domain Model

A commerce platform is easier to scale and maintain when its major business capabilities have clear responsibilities and boundaries.

The purpose of a domain model is not to create as many components as possible.

It is to answer a simpler question:

> **Which part of the platform is responsible for which business capability?**

---

## 1. Core Commerce Domains

A generalized enterprise commerce platform can be viewed through the following domains:

```text
                         COMMERCE
                            │
        ┌───────────────────┼───────────────────┐
        │                   │                   │
     Catalog            Customer             Pricing
        │                   │                   │
        └───────────────────┼───────────────────┘
                            │
                    Purchase Journey
                            │
                 ┌──────────┼──────────┐
                 │          │          │
               Cart      Checkout     Order
                 │          │          │
                 └──────────┼──────────┘
                            │
                    Fulfillment
                            │
                      Inventory
```

These domains interact, but they should not become indistinguishable from one another.

---

# 2. Product & Catalog

The product domain represents what the business sells.

Typical concepts include:

* products
* product variants
* categories
* catalogs
* classifications
* attributes
* media
* availability information
* merchandising relationships

A product model should distinguish between:

```text
Product
   │
   ├── Identity
   ├── Attributes
   ├── Classification
   ├── Media
   ├── Relationships
   └── Availability Context
```

Catalog structures may be optimized for different experiences without changing the underlying product ownership model.

---

# 3. Customer

The customer domain represents the commercial identity and context of the buyer.

Depending on the business model, this may include:

* individuals
* organizations
* business units
* customer groups
* addresses
* account relationships
* preferences
* permissions

B2B commerce often introduces additional complexity:

```text
Organization
    │
    ├── Business Unit
    │
    ├── Users
    │
    ├── Roles
    │
    ├── Cost Centers
    │
    └── Purchasing Rules
```

Customer context should not automatically become the responsibility of every downstream system.

---

# 4. Pricing

Pricing determines the commercial value associated with a product or transaction.

Possible inputs include:

* product
* customer
* customer segment
* quantity
* currency
* market
* contract
* effective date
* pricing rules

A simplified model:

```text
Product
   +
Customer Context
   +
Quantity
   +
Commercial Rules
   ↓
Effective Price
```

Pricing logic should have clearly defined ownership because inconsistent price calculations across channels can create significant business risk.

---

# 5. Promotions

Promotions represent incentives applied according to defined commercial rules.

Examples include:

* percentage discounts
* fixed discounts
* bundles
* threshold promotions
* coupons
* customer-specific offers

Promotion logic should remain distinguishable from base pricing.

```text
Base Price
     │
     ▼
Promotion Evaluation
     │
     ▼
Adjusted Commercial Value
```

Separating the concepts helps prevent increasingly complex pricing logic from becoming difficult to reason about.

---

# 6. Cart

The cart represents the customer's current purchase intent.

Typical responsibilities include:

* cart entries
* quantities
* selected products
* calculated totals
* applied promotions
* delivery context
* customer context

The cart is generally temporary state.

It should not be treated as an order until the appropriate transaction boundary has been crossed.

---

# 7. Checkout

Checkout transforms purchase intent into a validated transaction.

Typical responsibilities include:

```text
Cart
 ↓
Customer validation
 ↓
Address / delivery
 ↓
Pricing validation
 ↓
Promotion validation
 ↓
Inventory validation
 ↓
Payment
 ↓
Order creation
```

Not every implementation requires these steps to occur synchronously.

The architecture should distinguish between:

* information required immediately
* work that can happen asynchronously
* actions requiring external confirmation

---

# 8. Order

The order represents the commercial transaction after checkout.

Typical order responsibilities include:

* order identity
* customer
* order entries
* pricing snapshot
* taxes
* payment state
* delivery information
* order status
* lifecycle state

A useful principle is:

> **An order should preserve the commercial meaning of what was purchased at the time of transaction.**

Downstream systems may receive the order, but that does not automatically mean they should become the owner of the commerce transaction lifecycle.

---

# 9. Inventory

Inventory represents availability and stock-related information.

Depending on the architecture, inventory may be owned by:

* commerce
* ERP
* dedicated inventory systems
* warehouse platforms
* other operational systems

Commerce often needs inventory information without necessarily owning the underlying stock ledger.

This distinction is important.

```text
Inventory System
      │
      │ Availability
      ▼
Commerce
      │
      ▼
Customer Experience
```

---

# 10. Fulfillment

Fulfillment represents how an order is operationally delivered.

Possible capabilities include:

* warehouse allocation
* shipment creation
* delivery
* pickup
* tracking
* fulfillment status
* returns

A generalized lifecycle might look like:

```text
Order
  ↓
Allocation
  ↓
Fulfillment
  ↓
Shipment
  ↓
Delivery
```

The commerce platform may orchestrate parts of this lifecycle without owning every operational process.

---

# 11. Domain Relationships

The domains interact through defined business relationships.

```text
Customer
   │
   ├──────────────→ Pricing
   │
   └──────────────→ Cart
                       │
                       ▼
                    Checkout
                       │
                       ▼
                     Order
                    /     \
                   /       \
            Inventory     Fulfillment
```

The goal is not to eliminate relationships.

The goal is to **make them intentional**.

---

# 12. Avoiding Domain Leakage

A common architectural problem occurs when one domain starts owning responsibilities that belong elsewhere.

For example:

```text
Checkout
 ├── pricing
 ├── customer
 ├── inventory
 ├── notification
 ├── ERP integration
 ├── analytics
 └── fulfillment
```

This can turn checkout into an orchestration bottleneck.

A better model separates responsibilities and uses explicit integration boundaries.

---

# 13. Domain Events

Domains can communicate meaningful state changes through events.

Examples:

```text
ProductPublished
PriceChanged
CartSubmitted
OrderConfirmed
PaymentCaptured
ShipmentDispatched
```

Events allow downstream capabilities to react without tightly coupling every component to the internal implementation of another domain.

---

# 14. Domain Ownership Checklist

For each major domain, ask:

* Who owns the data?
* Who owns the business rules?
* Who can modify it?
* Which systems consume it?
* Which events does it publish?
* Which events does it consume?
* What happens when dependencies fail?
* What consistency level is required?

---

# 15. Final Principle

A good commerce domain model does not attempt to create perfect theoretical boundaries.

It creates boundaries that are **useful for the business, understandable for engineers, and stable enough to support change.**

> **Clear ownership reduces accidental complexity.**
