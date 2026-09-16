# B2B Commerce Architecture

B2B commerce is not simply B2C commerce with a different user interface.

Business purchasing introduces additional concepts around organizations, account structures, permissions, negotiated pricing, purchasing policies, approvals, and operational workflows.

The architecture must therefore account for both the **individual user** and the **business entity they represent**.

---

# 1. B2B Commerce Model

A simplified model:

```text
Organization
      │
      ├── Business Units
      │       │
      │       └── Users
      │
      ├── Commercial Agreements
      │
      ├── Purchasing Rules
      │
      └── Account Structure
```

The user interacting with the platform is not necessarily the economic owner of the transaction.

---

# 2. Identity vs Organization

A B2B architecture should distinguish:

```text
User
 ↓
Identity
 ↓
Organization Context
 ↓
Permissions / Roles
 ↓
Commerce Capabilities
```

For example, two users belonging to the same organization may have different permissions.

One may be allowed to:

* create carts

while another may be allowed to:

* approve orders

and another may be allowed to:

* manage organization settings.

---

# 3. Organizational Hierarchy

B2B organizations can contain multiple levels.

```text
Enterprise
   │
   ├── Region
   │    ├── Business Unit A
   │    └── Business Unit B
   │
   └── Region
        ├── Business Unit C
        └── Business Unit D
```

The architecture should avoid hard-coding assumptions about organizational depth where the business structure is expected to evolve.

---

# 4. Roles and Permissions

Authorization should consider both:

```text
Who is the user?
```

and:

```text
What organization/context is the user operating within?
```

A conceptual model:

```text
User
 │
 ├── Role
 │
 ├── Organization
 │
 └── Permission
       │
       ▼
Commerce Capability
```

Authorization should be enforced at the appropriate business boundary rather than relying exclusively on the user interface.

---

# 5. Contractual Pricing

B2B pricing frequently depends on commercial relationships.

Possible inputs include:

* organization
* contract
* customer segment
* product
* quantity
* market
* currency
* effective period

Conceptually:

```text
Product
   +
Organization
   +
Commercial Agreement
   +
Quantity
   ↓
Applicable Price
```

Pricing ownership should be explicit to prevent different systems from producing conflicting results.

---

# 6. Purchasing Policies

B2B purchasing may introduce constraints such as:

* minimum quantities
* spending limits
* approved products
* approval requirements
* organizational budgets
* purchasing schedules

A simplified flow:

```text
User
 ↓
Create Cart
 ↓
Validate Policy
 ↓
Approval Required?
 ├── No → Checkout
 │
 └── Yes → Approval Workflow
                ↓
             Checkout
```

Approval should be treated as a business capability rather than simply a UI feature.

---

# 7. Account-Based Commerce

B2B transactions often require the organization to remain visible throughout the purchase lifecycle.

```text
User
 ↓
Organization
 ↓
Cart
 ↓
Order
 ↓
Fulfillment
```

This context can affect:

* pricing
* permissions
* shipping
* payment terms
* reporting
* approval
* order visibility

---

# 8. B2B Integration Landscape

A B2B commerce platform may integrate with:

```text
                 Commerce
                    │
       ┌────────────┼────────────┐
       │            │            │
      ERP          CRM        Identity
       │            │            │
       ├────────────┼────────────┤
       │            │            │
    Inventory    Service     Finance
```

Integration boundaries should be explicit.

Not every business rule needs to be duplicated inside commerce simply because another system participates in the process.

---

# 9. B2B Self-Service

A mature B2B platform can move operational activities from manual processes into controlled digital workflows.

Examples:

* order history
* reorder
* account management
* invoice access
* delivery tracking
* service requests
* organization administration

The architecture should distinguish between:

```text
Information retrieval
```

and:

```text
Business-changing actions
```

The latter generally require stronger authorization and auditability.

---

# 10. B2B Design Considerations

Before implementing a B2B capability, consider:

* organizational hierarchy
* user roles
* account context
* pricing ownership
* approval requirements
* purchasing policies
* order visibility
* data access
* integration ownership
* audit requirements

---

# 11. Final Principle

B2B commerce is fundamentally about **commercial context**.

The platform must understand not only:

> "Who is this user?"

but also:

> "Which business entity are they acting for, what are they allowed to do, and under which commercial rules?"

> **Identity establishes who you are. Organizational context determines what you can do.**
