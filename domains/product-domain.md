# Product Domain

The product domain is the foundation of an enterprise commerce platform.

It is responsible for representing what an organization sells, how products are structured, how they are discovered, and which commercial contexts can make them available.

A strong product domain separates **product information** from concerns such as pricing, inventory, customer eligibility, and order processing.

---

## 1. Domain Responsibilities

The product domain typically manages:

* Product identity
* Product types
* Product variants
* Categories
* Catalogs
* Product attributes
* Product relationships
* Media references
* Product lifecycle
* Classification
* Search and discovery metadata
* Channel or market availability

It should not become the owner of unrelated commercial concerns.

For example:

```text
Product
├── Identity
├── Attributes
├── Classification
├── Relationships
├── Catalog Placement
└── Media

Separate Domains
├── Pricing
├── Inventory
├── Promotion
├── Customer
└── Order
```

This separation reduces coupling and makes the platform easier to evolve.

---

## 2. Product Identity

Every product should have a stable business identity.

Typical identifiers may include:

* Product code
* SKU
* Variant identifier
* External system identifier
* GTIN or equivalent market identifier

Identifiers should be treated deliberately because multiple systems may refer to the same product differently.

A common pattern is:

```text
Commerce Product ID
        │
        ├── ERP Product ID
        ├── PIM Product ID
        ├── Marketplace ID
        └── External Reference
```

The commerce platform should avoid making external identifiers the sole foundation of its internal model unless the integration contract explicitly requires it.

---

## 3. Product Types and Variants

Enterprise catalogs frequently contain different product structures.

Examples include:

* Simple products
* Variant products
* Configurable products
* Bundles
* Kits
* Service products
* Digital products

A variant relationship should represent meaningful commercial differences.

For example:

```text
Running Shoe
├── Size 8 / Black
├── Size 9 / Black
├── Size 10 / Black
├── Size 8 / Blue
└── Size 9 / Blue
```

Avoid creating unnecessary product variants when an attribute can represent the difference without creating another sellable entity.

---

## 4. Catalog and Category Boundaries

A catalog organizes product information for a specific commercial context.

The same product may appear differently across:

* Markets
* Regions
* Channels
* Customer segments
* Brands
* Business units

A useful conceptual model is:

```text
Product
   │
   ├── Catalog
   │      └── Category
   │
   ├── Channel
   │
   └── Market
```

Catalog structure should remain independent from navigation presentation wherever possible.

This allows the same underlying product information to support different experiences.

---

## 5. Product Relationships

Relationships can improve discovery and merchandising.

Examples:

* Related products
* Accessories
* Replacement products
* Alternatives
* Frequently purchased together
* Compatible products
* Parent/child products

Relationships should have explicit business meaning rather than becoming a generic mechanism for arbitrary associations.

---

## 6. Product Availability

Product availability is often confused with product existence.

A product may exist in the catalog while being unavailable for a particular:

* Market
* Customer
* Channel
* Store
* Date range
* Fulfillment method

Therefore:

```text
Product Exists
      ↓
Commercial Eligibility
      ↓
Inventory Availability
      ↓
Purchasable
```

These are separate decisions.

---

## 7. Search and Discovery

Search is usually a consumer of product information rather than the product domain itself.

A typical flow is:

```text
Product Data
     ↓
Indexing Pipeline
     ↓
Search Index
     ↓
Query
     ↓
Search Results
```

The search layer may derive:

* Searchable attributes
* Facets
* Sorting fields
* Ranking signals
* Suggestion data
* Category relationships

The source-of-truth product model should not be designed around the limitations of a particular search engine.

---

## 8. Product Lifecycle

Products often move through lifecycle states.

For example:

```text
Draft
  ↓
Approved
  ↓
Published
  ↓
Available
  ↓
Discontinued
  ↓
Archived
```

Lifecycle rules should be explicit.

Publishing a product should not automatically imply that it is:

* In stock
* Eligible for every customer
* Available in every market
* Correctly priced

Those concerns belong to their respective domains.

---

## 9. Design Principles

### Separate product information from commercial state

A product's description and attributes should not be tightly coupled to pricing or inventory.

### Prefer stable identifiers

Identifiers should remain stable even when presentation information changes.

### Keep source-of-truth boundaries explicit

If product information originates from another system, define synchronization ownership clearly.

### Avoid over-modeling

Not every attribute requires a new domain object.

### Design for multiple channels

The product model should support web, mobile, assisted commerce, APIs, marketplaces, and other channels without duplicating the core product.

---

## 10. Common Failure Modes

### Product model becomes the entire commerce model

**Problem:** Pricing, inventory, customer eligibility, and promotions become embedded into product entities.

**Impact:** High coupling and difficult change management.

### Search becomes the source of truth

**Problem:** Business logic begins depending directly on search-index structures.

**Impact:** Synchronization and data-consistency problems.

### Excessive variants

**Problem:** Every attribute combination becomes a separate product.

**Impact:** Catalog explosion and operational complexity.

### Unclear ownership

**Problem:** Multiple systems update the same product attributes.

**Impact:** Data conflicts and unpredictable synchronization.

---

## 11. Architectural Principle

> **The product domain should describe what the organization sells—not every condition under which it can be sold.**

A clean product domain provides a stable foundation for pricing, promotion, inventory, discovery, and order management without absorbing their responsibilities.
