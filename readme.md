# CodeWeave ADR Framework (Public Sample)

Stop debating boilerplate architecture and build systems that let you sleep at night.

This repository contains a **public sample of the CodeWeave Architecture Decision Record framework**.

CodeWeave is an engineering doctrine designed for teams who:

- operate without dedicated platform or SRE functions
- own systems end-to-end, including on-call responsibility
- expect systems to live longer than originally planned
- value predictable behaviour under operational stress

This repository intentionally includes **only a subset of the full ADR library**.

---

# Core Decisions (Public ADRs)

These decisions represent foundational engineering constraints used throughout the framework.

| ADR | Title | Status | Why it matters |
|---|---|---|---|
| [ADR-0-001](./adr/ADR-0-001-default-execution-model.md) | Default Execution Model | Accepted | Defines the single-host operational baseline |
| [ADR-3-012](./adr/ADR-3-012-context-preservation-strategy.md) | Context Preservation Strategy | Accepted | Ensures architectural reasoning is preserved |
| [ADR-4-015](./adr/ADR-4-015-configuration-management.md) | Configuration Management | Accepted | Standardises runtime configuration |
| [ADR-4-024](./adr/ADR-4-024-error-handling-classification.md) | Error Handling & Classification | Accepted | Creates predictable failure behaviour |
| [ADR-4-025](./adr/ADR-4-025-structured-logging-strategy.md) | Structured Application Logging | Accepted | Enables reliable operational diagnostics |

---

# Read by Scenario

**I want to understand how the system is deployed**

→ Read [ADR-0-001](./adr/ADR-0-001-default-execution-model.md)

---

**I want to understand how architectural reasoning is preserved**

→ Read [ADR-3-012](./adr/ADR-3-012-context-preservation-strategy.md)

---

**I want to know how runtime configuration should work**

→ Read [ADR-4-015](./adr/ADR-4-015-configuration-management.md)

---

**I want to know how failures should be classified**

→ Read [ADR-4-024](./adr/ADR-4-024-error-handling-classification.md)

---

**I want to understand logging expectations**

→ Read [ADR-4-025](./adr/ADR-4-025-structured-logging-strategy.md)

---

# Decision Map

```mermaid
flowchart TD

A[ADR-0-001 Default Execution Model]
B[ADR-3-012 Context Preservation Strategy]
C[ADR-4-015 Configuration Management]
D[ADR-4-024 Error Classification]
E[ADR-4-025 Structured Logging]

A --> C
A --> D
C --> D
D --> E
B --> A
B --> C
B --> D
B --> E