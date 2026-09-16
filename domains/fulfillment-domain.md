# Fulfillment Domain

Fulfillment converts a confirmed order into physical or digital delivery.

It connects commerce with operational systems responsible for:

- Warehouses
- Stores
- Carriers
- Delivery services
- Suppliers
- Distribution networks

The commerce platform should coordinate fulfillment without attempting to become the warehouse management system or carrier platform.

---

## 1. Domain Responsibilities

Fulfillment typically manages:

- Fulfillment requests
- Fulfillment locations
- Allocation
- Shipment creation
- Delivery options
- Shipment tracking references
- Fulfillment status
- Partial fulfillment
- Cancellation coordination

---

## 2. Fulfillment Flow

A simplified model:

```text
Confirmed Order
      ↓
Fulfillment Planning
      ↓
Inventory Allocation
      ↓
Fulfillment Request
      ↓
Pick / Pack
      ↓
Shipment
      ↓
Delivery
```

Each stage may be owned by a different system.

---

## 3. Fulfillment Strategies

Common models include:

### Single-location fulfillment

One location fulfills the order.

### Multi-location fulfillment

Different items are fulfilled from different locations.

```text
Order
├── Item A → Warehouse
├── Item B → Store
└── Item C → Supplier
```

### Ship-from-store

A retail store becomes a fulfillment location.

### Drop shipment

A supplier or partner fulfills the item directly.

The architecture should support these models without forcing every order through the same workflow.

---

## 4. Allocation

Allocation determines where demand should be fulfilled from.

Potential factors include:

- Inventory availability
- Customer location
- Shipping cost
- Delivery promise
- Warehouse capacity
- Product restrictions
- Business priorities

Allocation should be modeled as a business capability rather than hidden inside order processing.

---

## 5. Delivery Promise

Customers often care about:

> When will I receive this?

This requires more than inventory availability.

Conceptually:

```text
Inventory
+
Location
+
Processing Time
+
Carrier Capability
+
Destination
        ↓
Delivery Promise
```

The promise should be based on information appropriate to the required accuracy.

---

## 6. Partial Fulfillment

An order may be fulfilled in multiple stages.

Example:

```text
Order
 ↓
Shipment A → Delivered
 ↓
Shipment B → In Transit
```

The order should not be incorrectly marked as fully completed simply because one shipment succeeded.

---

## 7. External Fulfillment Systems

Commerce may integrate with:

- Warehouse management systems
- ERP
- Order management systems
- Carrier platforms
- Third-party logistics providers

Integration should be resilient to:

- Delayed responses
- Duplicate messages
- Temporary failures
- Partial processing

---

## 8. Fulfillment Events

Useful events may include:

```text
FulfillmentCreated
InventoryAllocated
ShipmentCreated
ShipmentDispatched
ShipmentDelivered
FulfillmentFailed
```

Events allow downstream systems to react without requiring every process to remain synchronously coupled.

---

## 9. Common Failure Modes

### Commerce owns warehouse logic

**Problem:** Commerce attempts to manage detailed operational warehouse processes.

**Impact:** Excessive scope and coupling.

### No partial fulfillment model

**Problem:** Orders are treated as atomic shipments.

**Impact:** Incorrect status and customer communication.

### Delivery promise ignores operational reality

**Problem:** Availability is treated as equivalent to deliverability.

**Impact:** Incorrect customer expectations.

### Integration assumes synchronous availability

**Problem:** Fulfillment systems must respond immediately.

**Impact:** Fragility during downstream outages.

---

## 10. Architectural Principle

> **Commerce should coordinate the customer-facing fulfillment journey while operational systems remain authoritative for operational execution.**