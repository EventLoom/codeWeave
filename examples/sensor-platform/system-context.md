# System Context: Industrial Sensor Monitoring Platform

## 1. System Overview

The **Industrial Sensor Monitoring Platform** is a critical operational system that ingests telemetry data from factory floor sensors, evaluates thresholds in real time, and triggers alerts when machinery operates outside safe parameters.

Because we expect systems to live longer than originally planned—and because we operate without dedicated platform or SRE functions—this system is designed to be **boring, legible, and robust**.

We deliberately reject hypothetical future scale in favour of operational clarity. The existence of this document itself is a requirement of our governance plane to ensure that the next engineer doesn't have to guess why we built it this way.

**Governing Decision:**  
[ADR-3-012 — Context Preservation Strategy](../../adr/ADR-3-012-context-preservation-strategy.md)

Architectural decisions are explicitly documented with rationale and trade-offs so that system intent remains visible to future engineers.

---

# 2. Structural Architecture

To prevent premature distributed complexity, the system is designed with explicit boundaries and minimal moving parts.

We gladly accept the trade-off of a **single point of failure and limited horizontal scaling** in exchange for **massive operational simplicity and reduced coordination complexity**.

### Execution Environment

The system runs on a **single Linux host**, which serves as the unit of failure, deployment, and operational responsibility.

**Governing Decision:**  
[ADR-0-001 — Default Execution Model](../../adr/ADR-0-001-default-execution-model.md)

By keeping the operational boundary constrained to a single host, we dramatically reduce coordination complexity and the number of failure modes that can emerge during incidents.

---

# 3. Engineering Standards & Operations

The cost of a decision is paid when something goes wrong, not when everything is healthy.

Our operational setup and engineering standards are strictly defined so that failures are **classifiable and behaviour is observable**.

### Configuration

All runtime configuration is managed via **environment variables**.

This guarantees simplicity and auditability over the hidden coupling of complex configuration files.

**Governing Decision:**  
[ADR-4-015 — Configuration Management](../../adr/ADR-4-015-configuration-management.md)

Examples of runtime configuration include:

- sensor ingestion endpoints  
- alerting thresholds  
- notification targets  
- database connection strings

---

### Error Handling

When a sensor payload is malformed or a threshold evaluation fails, the system enforces a **consistent taxonomy and representation for failures**.

This produces predictable and diagnosable failure behaviour.

**Governing Decision:**  
[ADR-4-024 — Error Handling & Classification Model](../../adr/ADR-4-024-error-handling-classification-model.md)

Examples of failure classifications include:

- `SensorPayloadMalformed`
- `SensorTimeout`
- `ThresholdEvaluationFailure`
- `AlertDispatchFailure`

This standardised taxonomy allows failures to be **diagnosed quickly during incidents**.

---

### Observability

The platform emits **structured logs for all operational events**.

Each log entry contains consistent metadata fields so that events remain machine-parseable and easy to correlate during operational investigations.

Typical structured log fields include:

- timestamp  
- sensor ID  
- processing stage  
- threshold evaluation result  
- error classification (if applicable)

**Governing Decision:**  
[ADR-4-025 — Structured Application Logging Strategy](../../adr/ADR-4-025-structured-application-logging-strategy.md)

Structured logs ensure that system behaviour remains:

- observable  
- diagnosable  
- automation-friendly.

---

# 4. Framework Scope

This repository intentionally includes **only a subset of the CodeWeave Architecture Decision Record library**.

The five ADRs referenced here demonstrate the **core operational philosophy of the framework**, but they do not represent the complete decision set used in production environments.

Additional decisions governing areas such as:

- security boundaries  
- dependency management  
- backup strategies  
- operational control  
- deployment discipline  
- incident response  

exist within the **full CodeWeave ADR Framework**.

---

# 5. Summary of Trade-offs

If this system were built to maximise architectural ambition, it would likely use:

- Kubernetes
- event-driven microservices
- autoscaling clusters
- distributed tracing infrastructure

We deliberately chose **not** to adopt these patterns.

Instead, the system is constrained by a small number of explicit architectural rules that optimise for:

- operational clarity  
- maintainability  
- diagnosable failures.

Constraints are easier to reason about than hidden coupling.

This architecture is **intentionally simple, operationally legible, and governed by the CodeWeave ADR framework**.