# Enterprise Commerce Architecture Review Checklist

## Purpose

This checklist provides a practical framework for reviewing an enterprise commerce solution before implementation or major architectural change.

It is intentionally technology-neutral and can be applied to new platforms, modernization programs, integrations, and major feature initiatives.

---

## 1. Business Context

- [ ] Business objective is clearly defined
- [ ] Target customer/channel is identified
- [ ] Business constraints are documented
- [ ] Regulatory or compliance requirements are known
- [ ] Expected scale is understood
- [ ] Critical business journeys are identified
- [ ] Success measures are defined

---

## 2. Domain Architecture

- [ ] Major business capabilities are identified
- [ ] Domain ownership is explicit
- [ ] Responsibilities do not overlap unnecessarily
- [ ] Business rules have clear ownership
- [ ] Data ownership is documented
- [ ] Cross-domain dependencies are understood
- [ ] Domain boundaries can evolve independently

---

## 3. API Architecture

- [ ] APIs represent meaningful business capabilities
- [ ] Contracts are documented
- [ ] Authentication is defined
- [ ] Authorization is defined
- [ ] Input validation exists
- [ ] Error handling is standardized
- [ ] Versioning strategy exists
- [ ] Rate limits are understood
- [ ] API performance requirements are defined

---

## 4. Event Architecture

- [ ] Events represent meaningful business occurrences
- [ ] Event ownership is explicit
- [ ] Event contracts are documented
- [ ] Consumers are idempotent
- [ ] Duplicate delivery is considered
- [ ] Ordering requirements are known
- [ ] Retry strategy exists
- [ ] Dead-letter handling exists where appropriate
- [ ] Event versioning is considered

---

## 5. Data Architecture

- [ ] Source of truth is defined
- [ ] Data ownership is clear
- [ ] Replication is intentional
- [ ] Data lifecycle is understood
- [ ] Historical transaction data is protected from mutable master data
- [ ] Database access patterns are understood
- [ ] Indexing strategy is appropriate
- [ ] High-volume queries have been considered

---

## 6. Performance

- [ ] Performance requirements are measurable
- [ ] Expected throughput is understood
- [ ] Latency targets are defined
- [ ] p95/p99 behavior is considered
- [ ] Database bottlenecks are considered
- [ ] Search performance is considered
- [ ] Cache strategy is defined
- [ ] Large payloads are avoided
- [ ] N+1 access patterns are checked
- [ ] Load testing strategy exists

---

## 7. Scalability

- [ ] Horizontal scaling is possible where required
- [ ] Stateful dependencies are identified
- [ ] Database scaling strategy exists
- [ ] Search scaling is considered
- [ ] Asynchronous processing is used where appropriate
- [ ] Traffic spikes are considered
- [ ] Background workloads are isolated where necessary

---

## 8. Resilience

- [ ] External dependencies have timeouts
- [ ] Retry policies are explicit
- [ ] Retryable and non-retryable failures are distinguished
- [ ] Circuit breaking is considered
- [ ] Graceful degradation is defined
- [ ] Bulkhead isolation is considered
- [ ] Compensation mechanisms exist where required
- [ ] Unknown transaction outcomes are handled

---

## 9. Security

- [ ] Authentication is defined
- [ ] Authorization is contextual where necessary
- [ ] Least privilege is applied
- [ ] Sensitive data is identified
- [ ] APIs are protected
- [ ] Administrative interfaces are protected
- [ ] Secrets are managed securely
- [ ] Audit requirements are understood
- [ ] Security events are observable

---

## 10. Integration

- [ ] Integration purpose is documented
- [ ] Producer and consumer are known
- [ ] Data ownership is clear
- [ ] API vs event decision is intentional
- [ ] Failure behavior is defined
- [ ] Idempotency is addressed
- [ ] External dependencies are monitored
- [ ] Contract evolution is considered

---

## 11. Upgradeability

- [ ] Standard platform capabilities are preferred
- [ ] Customization is justified
- [ ] Extension points are understood
- [ ] Upgrade impact has been considered
- [ ] Deprecated APIs or components are identified
- [ ] Vendor-supported mechanisms are preferred
- [ ] Technical debt introduced by the solution is documented

---

## 12. Operational Readiness

- [ ] Logs are structured
- [ ] Metrics exist
- [ ] Distributed tracing is available where appropriate
- [ ] Correlation IDs are propagated
- [ ] Alerts are defined
- [ ] Dashboards exist
- [ ] Operational ownership is clear
- [ ] Failure recovery procedures exist

---

## 13. Cost & Complexity

- [ ] New infrastructure has a clear purpose
- [ ] Operational overhead is understood
- [ ] Licensing implications are understood
- [ ] Additional services are justified
- [ ] Architectural complexity is measurable
- [ ] Simpler alternatives were considered

---

## 14. AI Considerations

Where AI is introduced:

- [ ] AI use case has a clear business purpose
- [ ] Authoritative data sources are identified
- [ ] Grounding strategy is defined
- [ ] User authorization is enforced
- [ ] Tool access is restricted
- [ ] Sensitive information controls exist
- [ ] Prompt injection risks are considered
- [ ] Output validation exists
- [ ] Human approval exists for consequential actions
- [ ] AI quality can be evaluated
- [ ] AI-specific observability exists
- [ ] Failure behavior is defined

---

## 15. Final Architecture Questions

Before approving an architecture, ask:

### Ownership

> Who owns each important capability and data set?

### Failure

> What happens when each dependency fails?

### Scale

> What happens when traffic or data volume increases by 10×?

### Change

> What happens when the business rule changes?

### Upgrade

> What happens when the underlying platform is upgraded?

### Security

> Who is allowed to perform each sensitive operation?

### Observability

> Can production teams understand what happened without guessing?

### Complexity

> Is every major component earning its place?

---

## Architecture Review Outcome

The objective of an architecture review is not to eliminate all risk.

It is to make important decisions **explicit, measurable, and reversible where possible**.

A review should produce:

```text
Architecture
    ↓
Known Decisions
    ↓
Known Trade-offs
    ↓
Known Risks
    ↓
Mitigation / Ownership
    ↓
Review Conditions
```

---

## Guiding Principle

> **Good architecture is not the absence of complexity. It is complexity that has a reason, an owner, and a boundary.**