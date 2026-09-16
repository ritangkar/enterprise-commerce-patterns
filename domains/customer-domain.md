# Customer Domain

The customer domain represents the people and organizations interacting with a commerce platform.

Enterprise commerce frequently needs to support multiple customer models simultaneously:

* Individual consumers
* Business organizations
* Employees
* Partners
* Buyers
* Administrators
* Guest users

The architecture should distinguish **identity**, **customer profile**, **organization structure**, and **commercial context**.

---

## 1. Domain Responsibilities

The customer domain typically manages:

* Customer identity references
* Customer profiles
* Organizations
* Contacts
* Roles
* Relationships
* Addresses
* Customer preferences
* Account status
* Customer segmentation references

It should not become the owner of unrelated concerns such as:

* Product information
* Pricing rules
* Order fulfillment
* Payment processing

---

## 2. Identity vs Customer

Authentication and customer identity are related but not identical.

A useful conceptual separation is:

```text
Identity
   ↓
Authenticated Actor
   ↓
Customer Context
   ↓
Commerce Permissions
```

An identity system may determine **who the actor is**.

The commerce platform determines what that actor is allowed to do within the commerce context.

This distinction becomes particularly important in B2B commerce.

---

## 3. B2C Customer Model

A typical B2C customer may have:

```text
Customer
├── Identity
├── Profile
├── Addresses
├── Preferences
├── Orders
└── Loyalty / Membership References
```

The commerce platform should avoid storing unnecessary identity information when another authoritative identity system already manages it.

---

## 4. B2B Organization Model

B2B commerce introduces organizational relationships.

For example:

```text
Organization
├── Parent Organization
├── Business Units
├── Buyers
├── Approvers
├── Administrators
└── Locations
```

A user may therefore have multiple contexts.

Example:

```text
User
  ↓
Organization
  ↓
Business Unit
  ↓
Role
  ↓
Purchasing Permissions
```

The active organization context may influence:

* Catalog access
* Pricing
* Payment terms
* Order limits
* Approval workflows
* Delivery locations

---

## 5. Roles and Permissions

Roles should describe business capabilities rather than merely technical implementation details.

Examples:

* Buyer
* Approver
* Account Administrator
* Purchaser
* Viewer

A permission model should answer:

> What can this actor do in this commercial context?

Rather than:

> Which technical endpoint can this user call?

Authorization should ultimately be enforced at the service boundary.

---

## 6. Customer Context

Customer context can influence many downstream decisions.

For example:

```text
Customer Context
      │
      ├── Product Eligibility
      ├── Pricing
      ├── Promotions
      ├── Payment Terms
      ├── Delivery Options
      └── Order Permissions
```

The important architectural principle is that these domains **consume customer context** rather than embedding customer logic independently.

---

## 7. Guest Commerce

Guest users introduce a separate identity state.

A guest may be allowed to:

* Browse
* Search
* Add products to cart
* Checkout
* Track an order using permitted information

The architecture should define explicitly what data can transition from guest context into registered customer context.

---

## 8. Customer Data Ownership

Enterprise environments frequently integrate multiple systems.

For example:

```text
Identity Provider
      │
      ├── Authentication
      │
CRM / Customer Platform
      │
      ├── Customer Profile
      │
Commerce Platform
      │
      ├── Commerce Context
      └── Transactional State
```

Ownership should be explicit.

A commerce platform should avoid becoming an accidental master for data owned elsewhere.

---

## 9. Privacy and Data Minimization

Customer data should be collected and retained according to actual business requirements.

Useful principles include:

* Minimize stored personal information
* Restrict access by role
* Avoid unnecessary duplication
* Protect sensitive attributes
* Define retention requirements
* Audit sensitive operations
* Separate authentication secrets from commerce data

---

## 10. Common Failure Modes

### Customer becomes a dumping ground

**Problem:** CRM, identity, commerce, loyalty, and service data are all stored in one model.

**Impact:** Tight coupling and unclear ownership.

### Roles become technical flags

**Problem:** Authorization is implemented as scattered boolean attributes.

**Impact:** Difficult-to-maintain permission logic.

### Organization context is ignored

**Problem:** B2B users are treated exactly like B2C customers.

**Impact:** Incorrect pricing, permissions, and order behavior.

### Customer data is duplicated everywhere

**Problem:** Multiple systems independently maintain profile data.

**Impact:** Synchronization conflicts and privacy risk.

---

## 11. Architectural Principle

> **Identity establishes who the actor is; customer context establishes how that actor participates in commerce.**

Keeping those concepts separate creates a cleaner foundation for both B2C and B2B commerce.
