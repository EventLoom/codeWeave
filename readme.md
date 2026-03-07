# CodeWeave ADR Framework (Sample)

**Stop debating boilerplate architecture and build systems that let you sleep at night.**

This repository contains a sample of the **CodeWeave engineering doctrine**. This framework is not a catalogue of best practices, a reference architecture, or a cloud-native maturity ladder.

Instead, it is a **hard-won record of decisions** that prioritize **operational clarity, failure recoverability, and long-term maintainability** over architectural ambition. It provides a consistent foundation for engineering teams by converting implicit engineering culture into explicit policy.

---

# Why This Framework?

Most architectural frameworks optimize for hypothetical future scale and success. **CodeWeave optimizes for failure.**

We accept that systems are operated by people, often under stress, and that **a system requiring heroics to operate is fundamentally fragile.**

Therefore, this operating model is intentionally biased toward:

- **Predictability over elasticity**
- **Explicit trade-offs over implicit decisions**
- **Controlled scaling over speculative scaling**
- **Operational clarity over architectural fashion**

This framework is explicitly designed for teams who:

- Operate **without dedicated platform or SRE functions**
- Own systems **end-to-end**, including on-call responsibility
- Expect systems to **live longer than originally planned**
- Value **predictable behaviour under stress**

---

# Opinionated Defaults, Not Blank Templates

Engineers don't need more blank templates; they need **decisions that constrain architectural choice to what is operationally tractable**.

The CodeWeave framework prefers **explicit constraints to implicit complexity** because constraints reduce the **surface area of failure**.

The full, expanded, library contains **35+ decisions** that deliberately slow certain forms of complexity introduction.

### Example decisions include:

- **Avoiding orchestration platforms**  
  Defaulting to a single Linux host and avoiding Kubernetes by default to reduce coordination complexity.

- **Rejecting premature decomposition**  
  Avoiding microservices by default and preferring monolithic deployments for simpler failure modes.

- **Offloading state management**  
  Defaulting to managed database services over self-hosted options to reduce operational risk.

- **Prioritizing maintainability**  
  Explicitly optimising for maintainability over performance, accepting potentially lower performance for long-term flexibility.

By making these trade-offs explicit, CodeWeave helps teams avoid repeating the same design debates and ensures that software remains:

- **Reasonable**
- **Evolvable**
- **Observable**
- **Recoverable**

---

# The Operating Model Structure


CodeWeave is **not a linear maturity ladder**.

It is an **integrated operating system for engineering teams**.

The full framework is composed of **parallel domains of concern**, constrained by a strict governance control plane:

[![CodeWeave Decision Constellation](docs/decision-constellation.svg)](https://codeweave.lemonsqueezy.com/checkout)

### Structural Architecture (Layers 0–2)

Defines **what the system is and how it runs**.

Includes:

- Execution models
- Trust boundaries
- A highly conservative **capacity and scaling philosophy**

---

### Engineering Standards (Layer 4)

Defines **how the system is built and evolved**.

Includes discipline around:

- API design
- Structured logging
- Migration strategy

---

### Incident Response (Layer 5)

Assumes **failures will occur**.

Prioritizes:

- Predictable recovery
- Service restoration

over immediate root cause analysis.

---

### Governance Control Plane (Layer 3)

The **cross-cutting enforcement mechanism**.

Prevents gradual erosion by ensuring:

- Breaking changes are **intentional**
- Scaling complexity is **actually justified**

---

# What's Inside This Sample?

This repository gives you a **taste of the CodeWeave doctrine**.

We aren't just giving you blank templates — we are giving you **5 battle-tested decisions from the full library** to show you exactly how this framework curtails accidental complexity.

---

# Included Architecture Decision Records (ADRs)

### ADR-0-001 — Default Execution Model

Mandates defaulting to a **single Linux host as the unit of failure**, explicitly trading a single point of failure for massive operational simplicity.

---

### ADR-3-012 — Context Preservation Strategy

Establishes the rule for **explicitly documenting architectural decisions**, forcing teams to weigh documentation overhead against preserved intent.

---

### ADR-4-015 — Configuration Management

Mandates the use of **environment variables for all runtime configuration**, optimizing for simplicity and auditability.

---

### ADR-4-024 — Error Handling & Classification Model

Defines a **consistent taxonomy for failures across services**, trading upfront design effort for predictable and diagnosable failure behaviour.

---

### ADR-4-025 — Structured Application Logging Strategy

Standardizes **structured logging** to ensure consistent diagnostics and operational clarity, making your systems **automation-ready**.

---

# The Example System: Seeing It in Practice

Architecture decisions don't exist in a vacuum.

To demonstrate how these constraints compound to create a **highly legible, robust system**, we've included a fictional:

**Industrial Sensor Monitoring Platform**

This example includes:

- A system context description
- A project-specific ADR index
- Architectural documentation

This demonstrates how **CodeWeave principles apply to real-world design constraints**.

---

# The Power of the Template

Every system eventually reflects the decisions that were **easiest to make under time pressure**.

The CodeWeave templates are designed to stop that from happening.

The templates enforce our core philosophy:

Evaluate decisions by:

- **How they fail**
- **How obvious the failure is**
- **How much coordination is required under pressure**

Each decision forces teams to document:

- The conditions under which the decision applies
- The failure modes it introduces
- The exact cost of reversing the decision later

---

# Stop Re-litigating the Same Arguments

If a decision is challenged, the **burden of proof lies with the alternative**.

Re-litigation without new information is an **operational cost**.

---

# Get the Full CodeWeave ADR Framework

The complete framework contains **35+ architecture decisions** covering:

- System design principles
- Operational architecture
- Security boundaries
- Observability
- Deployment
- Governance control

Stop debating boilerplate.

**Buy the framework, set the baseline, and get back to building.**

➡️ **https://codeweave.lemonsqueezy.com/checkout**

**https://www.eventloomtech.com/codeweave**

---

# License

The contents of this repository are provided for **personal and internal organisational use**.

They **may not be redistributed, sold, or republished** as part of another documentation framework, template library, or commercial product.

See the **LICENSE** file for full details.