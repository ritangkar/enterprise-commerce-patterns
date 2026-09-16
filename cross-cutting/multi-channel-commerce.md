# Multi-Channel Commerce

Enterprise commerce increasingly operates across multiple customer and operational channels.

Examples include:

- Web
- Mobile
- Mobile web
- Store
- Assisted selling
- Customer service
- Marketplace
- Partner portals
- APIs

The challenge is to provide consistent commerce capabilities without forcing every channel into an identical experience.

---

## 1. Channel vs Commerce Capability

A useful distinction is:

```text
Channels
├── Web
├── Mobile
├── Store
├── Service
└── Partner

        ↓

Shared Commerce Capabilities
├── Product
├── Pricing
├── Cart
├── Order
├── Customer
└── Inventory
```

Channels consume capabilities.

They should not independently recreate core commerce rules.

---

## 2. Headless Architecture

A headless approach separates the presentation layer from commerce capabilities.

```text
Web / Mobile / Store UI
          ↓
      Commerce APIs
          ↓
   Commerce Services
          ↓
     Enterprise Systems
```

This enables different experiences to consume shared capabilities.

---

## 3. Experience Independence

Different channels may require different experiences.

For example:

### Web

Optimized for:

- Discovery
- Merchandising
- Self-service

### Store

Optimized for:

- Speed
- Assisted selling
- Inventory visibility
- Customer lookup

### Customer Service

Optimized for:

- Context
- Case resolution
- Order management

The underlying business rules can remain shared.

---

## 4. Omnichannel Context

Customers increasingly expect continuity across channels.

A journey may look like:

```text
Web
 ↓
Product Discovery
 ↓
Store
 ↓
Purchase
 ↓
Mobile
 ↓
Order Tracking
 ↓
Customer Service
```

The architecture should preserve appropriate customer and transaction context across these transitions.

---

## 5. Shared Commerce State

Common shared state may include:

- Customer
- Cart
- Orders
- Product information
- Inventory availability
- Promotions

However, not every channel needs every piece of data.

Expose the minimum required capability through appropriate APIs.

---

## 6. Channel-Specific Composition

A channel may compose multiple capabilities.

For example:

```text
Store Application
      ↓
Customer Context
      +
Product
      +
Inventory
      +
Pricing
      +
Order History
```

The channel should orchestrate the user experience without taking ownership of those domains.

---

## 7. B2B Multi-Channel Complexity

B2B adds additional context:

```text
User
 +
Organization
 +
Business Unit
 +
Channel
 +
Permissions
```

The same user may receive different capabilities depending on the active business context.

---

## 8. Consistency vs Uniformity

Multi-channel commerce does not mean every channel must behave identically.

The goal is:

> **Consistent business rules with channel-appropriate experiences.**

For example, a store associate may require functionality that should never appear in a consumer storefront.

---

## 9. Common Failure Modes

### Duplicate business logic

**Problem:** Each channel implements pricing or promotion rules independently.

**Impact:** Inconsistent outcomes.

### Channel becomes domain owner

**Problem:** Mobile or web application becomes responsible for business state.

**Impact:** Difficult reuse.

### Over-centralized experience

**Problem:** Every channel is forced through the exact same UI and workflow.

**Impact:** Poor channel fit.

### Inconsistent customer context

**Problem:** Different channels see different transaction states.

**Impact:** Broken omnichannel experience.

---

## 10. Architectural Principle

> **Multi-channel commerce should share business capabilities while allowing each channel to optimize the experience for its users and operational context.**