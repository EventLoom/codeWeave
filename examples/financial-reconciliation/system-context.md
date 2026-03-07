# System Context: Internal Financial Reconciliation Engine

## 1. System Overview

The **Internal Financial Reconciliation Engine** is a backend system that runs nightly batch processes to match external payment gateway settlements against our internal ledger.

Because this system handles sensitive financial data, it is designed to **optimise for failure, not success**. If the reconciliation fails, the failure must be obvious, and recovery must occur quickly without complex coordination. We have explicitly chosen a design that is **boring, legible, and robust**.

**Governing Decision:**  
[ADR-3-011: Optimisation Target — Optimising for maintainability over performance](../../adr/ADR-3-011-optimisation-target.md)

We gladly accept that a batch job might take 20 minutes longer to run if the code remains simpler to debug during an incident.

**Governing Decision:**  
[ADR-3-012: Context Preservation Strategy — Explicitly documenting architectural decisions with rationale and trade-offs](../../adr/ADR-3-012-context-preservation.md)

---

## 2. Structural Architecture (Layers 0–2)

We reject the temptation to build a distributed, event-driven pipeline for this batch process. We avoid microservices by default, preferring monolithic deployments to ensure simpler failure modes.

### Execution Environment

The engine operates on a **single Linux host**, which acts as the unit of failure, deployment, and operational responsibility.

**Governing Decision:**  
[ADR-0-001: Default Execution Model](../../adr/ADR-0-001-default-execution-model.md)

### Operational Control

We use **systemd (specifically systemd timers)** for service management and scheduling over application-level process managers or external job schedulers (such as Airflow) to maintain operational consistency.

**Governing Decision:**  
[ADR-2-008: Operational Control Strategy](../../adr/ADR-2-008-operational-control.md)

### Data Layer

We default to **managed database services** over self-hosted options. Financial data requires absolute restore confidence, so we also prioritize simple, tested backups over complex automation.

**Governing Decisions:**

- [ADR-0-003: Database Ownership Model](../../adr/ADR-0-003-database-ownership.md)
- [ADR-3-009: Backup Strategy](../../adr/ADR-3-009-backup-strategy.md)

---

## 3. Security & Audit Posture (Layers 1 & 4)

Because this is a financial system, **regulatory clarity is paramount**. We heavily restrict the attack surface and ensure that security events are treated differently from standard operational noise.

### Network & Access

We maintain strict separation between **internal and public systems**. This engine has **no public ingress**. Authentication is intentionally simple, accepting limited federation in exchange for a reduced attack surface.

**Governing Decisions:**

- [ADR-1-007: Trust Boundaries](../../adr/ADR-1-007-trust-boundaries.md)
- [ADR-1-006: Authentication Model](../../adr/ADR-1-006-authentication-model.md)

### Compliance Tracking

We capture **security and compliance-relevant events separately from operational logs**. We accept the trade-off of managing additional log streams to ensure regulatory clarity and traceability.

**Governing Decision:**  
[ADR-4-021: Audit Logging Strategy](../../adr/ADR-4-021-audit-logging.md)

---

## 4. Engineering Standards & Quality (Layer 4)

To ensure predictable evolution of software systems and prevent release instability, the development of this engine is **heavily constrained**.

### Dependency Management

Financial math libraries and external SDKs are **strictly controlled**. We use conservative dependency updates with **exact version pinning**. We accept manual update effort to guarantee production stability.

**Governing Decision:**  
[ADR-4-020: Dependency Management](../../adr/ADR-4-020-dependency-management.md)

### Testing

Because this engine integrates heavily with external gateway APIs and the database, we **prioritize integration tests over unit tests**. Slower test execution is an acceptable trade-off for catching real bugs.

**Governing Decision:**  
[ADR-4-016: Testing Strategy](../../adr/ADR-4-016-testing-strategy.md)