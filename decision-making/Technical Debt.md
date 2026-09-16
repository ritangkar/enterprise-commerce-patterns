# Technical Debt

Technical debt is the future cost created when a system accumulates implementation or architectural compromises.

Not all technical debt is bad.

A deliberate short-term compromise can be reasonable when its cost and repayment path are understood.

The problem is **unmanaged debt**.

---

## 1. Types of Technical Debt

Technical debt can exist at several levels.

### Code debt

Examples:

- Duplicated logic
- Poor abstractions
- Fragile implementations

### Architecture debt

Examples:

- Excessive coupling
- Incorrect service boundaries
- Synchronous dependencies

### Data debt

Examples:

- Poor data modeling
- Missing indexes
- Duplicate data ownership

### Integration debt

Examples:

- Undocumented interfaces
- Point-to-point integrations
- Legacy protocols

### Operational debt

Examples:

- Poor monitoring
- Manual deployment
- Missing recovery procedures

---

## 2. Intentional vs Unintentional Debt

### Intentional

A team knowingly chooses a simpler temporary implementation to meet an immediate business need.

### Unintentional

Debt accumulates through:

- Lack of ownership
- Poor design
- Organizational change
- Evolving requirements
- Repeated local fixes

The second category is often harder to detect.

---

## 3. Debt Has Interest

A useful model is:

```text id="t8tq0w"
Technical Debt
      ↓
Maintenance Cost
      ↓
Slower Delivery
      ↓
More Workarounds
      ↓
More Debt
```

This creates compounding cost.

---

## 4. Measuring Debt

Useful indicators include:

- Change failure rate
- Defect frequency
- Upgrade effort
- Build/deployment time
- Incident frequency
- Performance degradation
- Developer effort spent on workarounds

Debt should be connected to measurable engineering or business impact.

---

## 5. Debt Prioritization

Not every debt item deserves immediate attention.

Consider:

```text id="v7r1fr"
Business Impact
×
Frequency
×
Risk
×
Cost of Delay
```

For example, a rarely used low-risk component may require less attention than a heavily used checkout dependency.

---

## 6. Debt Register

A simple register can contain:

| Debt | Impact | Risk | Owner | Action |
|---|---|---|---|---|
| Legacy integration | High | High | Team A | Replace |
| Duplicate utility | Medium | Low | Team B | Consolidate |
| Missing monitoring | High | Medium | Team C | Instrument |

The goal is visibility rather than bureaucracy.

---

## 7. Debt Repayment

Debt can be reduced through:

- Refactoring
- Architecture changes
- Platform upgrades
- Integration consolidation
- Data cleanup
- Automation
- Observability improvements

Repayment should be incorporated into normal delivery planning where practical.

---

## 8. Avoiding Debt

Good architectural habits reduce future debt:

- Clear ownership
- Small boundaries
- Explicit contracts
- Automated testing
- Observability
- Documentation of important decisions
- Minimal unnecessary customization

---

## 9. Common Failure Modes

### Debt is invisible

**Problem:** No one records known compromises.

### Debt is treated as purely technical

**Problem:** Business impact is ignored.

### Everything is labeled technical debt

**Problem:** The term becomes meaningless.

### Debt repayment is always deferred

**Problem:** Short-term delivery continually increases long-term cost.

---

## 10. Architectural Principle

> **Technical debt is manageable when it is visible, intentional, owned, and periodically repaid.**