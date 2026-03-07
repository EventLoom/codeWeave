# System Context: Industrial Sensor Monitoring Platform

## 1. System Overview

The **Industrial Sensor Monitoring Platform** is a critical operational system that ingests telemetry data from factory floor sensors, evaluates thresholds in real time, and triggers alerts when machinery operates outside safe parameters.

Because we expect systems to live longer than originally planned—and because we operate without dedicated platform or SRE functions—this system is designed to be **boring, legible, and robust**.

We deliberately reject hypothetical future scale in favour of operational clarity. The existence of this document itself is a requirement of our governance plane to ensure that the next engineer doesn't have to guess why we built it this way.

**Governing Decision:** ADR-3-012 — *Context Preservation Strategy*  
Explicitly documenting architectural decisions with rationale and trade-offs.

**Governing Decision:** ADR-3-011 — *Optimisation Target*  
Optimising for maintainability over performance.

---

# 2. Structural Architecture (Layers 0–2)

To prevent premature distributed complexity, the system is designed with explicit boundaries and minimal moving parts.

We gladly accept the trade-off of a **single point of failure and limited horizontal scaling** in exchange for **massive operational simplicity and reduced coordination complexity**.

### Execution Environment

The system runs on a **single Linux host**, which serves as the unit of failure, deployment, and operational responsibility.

**Governing Decision:** ADR-0-001 — *Default Execution Model*

### Compute & Decomposition

The application is deployed as a **single monolithic service**.

We avoid microservices by default to prevent large codebases from fracturing into complex failure modes.

**Governing Decision:** ADR-0-004 — *Service Decomposition Strategy*

### Supervision

We use **systemd for service management** rather than application-level process managers.

This ensures operational consistency despite a steeper learning curve.

**Governing Decision:** ADR-2-008 — *Operational Control Strategy*

### Data Persistence

We default to **managed database services** (e.g., AWS RDS) rather than self-hosted databases.

We accept higher costs and vendor dependency in exchange for significantly reduced operational risk.

**Governing Decision:** ADR-0-003 — *Database Ownership Model*

---

# 3. Engineering Standards & Operations (Layer 4)

The cost of a decision is paid when something goes wrong, not when everything is healthy.

Our operational setup and engineering standards are strictly defined so that failures are **classifiable and behaviour is observable**.

### Configuration

All runtime configuration is managed via **environment variables**.

This guarantees simplicity and auditability over the hidden coupling of complex configuration files.

**Governing Decision:** ADR-4-015 — *Configuration Management*

### Error Handling

When a sensor payload is malformed, the system enforces a **consistent taxonomy and representation for failures**.

This produces predictable and diagnosable failure behaviour.

**Governing Decision:** ADR-4-024 — *Error Handling and Classification Model*

### Observability

We prioritise **metrics and structured logs** over distributed tracing.

Standardising structured logging across the service ensures consistent diagnostics and machine-parseable output for operational clarity.

**Governing Decisions:**

- ADR-4-025 — *Structured Application Logging Strategy*
- ADR-4-013 — *Monitoring and Observability Strategy*

### Data Evolution

All database migrations for sensor data schemas must be **backward compatible**.

**Governing Decision:** ADR-4-017 — *Data Migration Strategy*

---

# 4. Incident Response & Recovery (Layer 5)

When a failure occurs—such as a memory leak causing the sensor ingestion pipeline to stall—our incident discipline depends on the **structured logging, defined error codes, and deployment rollback capability** enforced elsewhere in the model.

### Incident Priority

During incidents, we prioritise **service restoration over root cause analysis**.

The goal is to minimise user impact, even if this introduces the risk of recurring incidents.

**Governing Decision:** ADR-5-018 — *Incident Response Model*

### Deployments & Rollback

We use **sequential, deliberate deployments with manual verification**.

We accept longer deployment times because they guarantee reliable rollback capability.

**Governing Decision:** ADR-4-014 — *Deployment Strategy*

---

# 5. Summary of Trade-offs

If this system were built to maximise architectural ambition, it would likely use:

- Kubernetes
- Event-driven microservices
- Autoscaling groups
- Distributed tracing

We deliberately chose **not** to adopt these patterns.

By keeping authentication intentionally simple to reduce the attack surface and optimising for **maintainability over performance**, we protect the team from on-call fatigue.

Constraints are easier to reason about than hidden coupling.

This architecture is **complete, closed, and strictly governed by the CodeWeave ADR framework**.

Any deviation or introduction of scaling complexity must be formally justified through the **Governance Control Plane**.