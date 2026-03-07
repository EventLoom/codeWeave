# ADR Index: Internal Financial Reconciliation Engine

## Purpose of this Document

This document lists the specific **Architecture Decision Records (ADRs)** from the global **CodeWeave framework** that govern the **Internal Financial Reconciliation Engine**.

This is **not a catalogue of best practices**. It is a record of our **active, binding constraints**. We deliberately limit architectural freedom in exchange for **clearer ownership, fewer coordination points, and shorter causal chains during incidents**.

If a decision here is challenged, the **burden of proof lies with the alternative**. Re-litigation without new information is considered an **operational cost**.

---

# 1. Foundation & Topology

Because this engine handles **batch reconciliation**, we reject distributed complexity in favor of **extreme operational legibility**.

---

## ADR-0-001 — Default Execution Model

**Status:** Active  

**Context:**  
Defaulting to a **single Linux host** as the unit of failure, deployment, and operational responsibility.

**Local Application:**  
The reconciliation engine runs as a **monolithic process on a single, isolated internal server**.

---

## ADR-0-003 — Database Ownership Model

**Status:** Active  

**Context:**  
Defaulting to **managed database services** over self-hosted.

**Local Application:**  
We use **AWS RDS** to ensure high availability of the ledger data, accepting **higher cost and vendor dependency** vs **reduced operational risk**.

---

## ADR-2-008 — Operational Control Strategy

**Status:** Active  

**Context:**  
Using **systemd** for service management over application-level process managers.

**Local Application:**  
We use **systemd timers** to trigger the nightly batch jobs instead of external orchestration tools like **Apache Airflow**, avoiding hidden failure modes.

---

# 2. Security & Compliance

Financial data requires **absolute clarity regarding access and auditability**.  
We optimize for **strict boundaries and regulatory traceability**.

---

## ADR-1-007 — Trust Boundaries

**Status:** Active  

**Context:**  
Maintaining **strict separation between internal and public systems**.

**Local Application:**  
This engine sits in a **private subnet with zero public ingress**.

---

## ADR-1-006 — Authentication Model

**Status:** Active  

**Context:**  
Keeping authentication **intentionally simple**.

**Local Application:**  
We accept **limited federation vs reduced attack surface**.  
The engine uses **strictly scoped, rotated service-account credentials** to access the database.

---

## ADR-4-021 — Audit Logging Strategy

**Status:** Active  

**Context:**  
Capturing **security and compliance-relevant events separately from operational logs**.

**Local Application:**  
All **ledger mismatches and data access events** are piped to a **hardened, append-only compliance log**, separate from standard application output.

---

# 3. Engineering & Maintainability

We accept that **"good enough" often outperforms "best"**.  
We optimize for code that is **boring to maintain over time**.

---

## ADR-3-011 — Optimisation Target

**Status:** Active  

**Context:**  
Optimising for **maintainability over performance**.

**Local Application:**  
If a batch reconciliation query takes slightly longer but is **easier to read and debug during an incident**, we accept the performance hit.

---

## ADR-4-016 — Testing Strategy

**Status:** Active  

**Context:**  
Prioritizing **integration tests over unit tests**.

**Local Application:**  
Slower test execution vs catching real bugs.

We spin up a **test database** and **mock the payment gateway** for end-to-end integration testing rather than unit testing isolated math functions.

---

## ADR-4-020 — Dependency Management

**Status:** Active  

**Context:**  
Conservative dependency updates with **exact version pinning**.

**Local Application:**  
No **floating versions** in our package manager.

We accept **manual update effort vs production stability** to ensure a third-party update doesn't silently break our financial calculations.

---

## ADR-3-009 — Backup Strategy

**Status:** Active  

**Context:**  
Preferring **simple, tested backups** over complex automation.

**Local Application:**  
We accept **manual effort vs restore confidence**.

Database **snapshots are tested manually every quarter**.

---

# 4. Documentation

---

## ADR-3-012 — Context Preservation Strategy

**Status:** Active  

**Context:**  
Explicitly documenting **architectural decisions with rationale and trade-offs**.

**Local Application:**  
This very document, along with **`system-context.md`**, serves as the required artifacts to satisfy this constraint.