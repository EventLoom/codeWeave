# ADR-0-001: Defaulting to a Single Linux Host

> CodeWeave Engineering Doctrine — Licensed for internal organisational use only. Redistribution or resale of this document as part of another framework, template library, or documentation product is not permitted.

## Status
Accepted

## Context
We needed to establish a default execution and deployment model for our systems.

Our systems typically exhibit the following characteristics:
- low to moderate request volume
- limited concurrency requirements
- a small number of tightly-coupled components
- operational ownership by a small team without dedicated infrastructure specialists
- long-lived deployments with infrequent architectural resets

We explicitly optimise for operational predictability, failure diagnosability, and reversibility over theoretical scalability.

## Decision
We default to deploying systems on a single Linux host.

This host is treated as:
- the unit of failure
- the unit of deployment
- the unit of operational responsibility

We avoid introducing additional hosts, clusters, or schedulers unless a concrete failure mode cannot be addressed within this model.

## Invariants
Under this decision, the following must remain true:
- every running process can be reasoned about from the host OS
- system state can be enumerated without consulting external control planes
- restart order and dependency chains are explicit
- recovery procedures can be executed by a single operator under time pressure

Any architecture that violates these invariants is considered a different class of system.

## Rationale
A single-host model minimises the number of independent failure domains.

This has several practical consequences:
- failure modes are dominated by hardware, OS, or application faults rather than coordination errors
- observability is grounded in first-order signals (CPU, memory, disk, network)
- causal chains during incidents are shorter and easier to reconstruct
- operational behaviour remains legible to engineers without specialised tooling knowledge

In practice, most outages at our scale are caused by configuration errors, unexpected state, or misunderstood dependencies rather than insufficient capacity.

The single-host model reduces exposure to these classes of failure.

## Failure Economics
Within this model:
- failures are typically total rather than partial
- detection is immediate and unambiguous
- recovery paths are linear (restart, restore, redeploy)
- mean-time-to-recovery is bounded by operator action, not system convergence

We explicitly prefer fast, obvious failures over degraded or ambiguous partial failure modes.

## Operational Implications
By committing to a single host:
- deployments are push-based and sequential
- rollback is a file- or process-level operation
- monitoring focuses on saturation and liveness, not orchestration health
- capacity planning is explicit and conservative
- incident response does not require cross-system coordination

This reduces cognitive load during incidents and lowers the risk of compounding failures through incorrect remediation.

## Rejected Alternatives
We considered:
- multi-host active/active deployments
- clustering at the application layer
- early adoption of orchestration platforms

These were rejected because they introduce:
- additional state replication concerns
- partial failure modes that are difficult to detect
- longer recovery times due to coordination overhead
- architectural commitments that are costly to reverse

At our current scale, these alternatives increase operational risk faster than they reduce availability risk.

## Trade-offs
This decision implies:
- a single point of failure at the host level
- vertical scaling as the primary capacity mechanism
- maintenance windows that may involve full service interruption

We accept these trade-offs because they are visible, predictable, and operationally tractable.

## When This Decision Might Change
This decision should be revisited only if:
- recovery time objectives cannot be met with a single host
- workload characteristics require isolation at the scheduling level
- failure impact exceeds acceptable business thresholds despite mitigations
- operational complexity from vertical scaling exceeds that of coordination

Anticipated growth or architectural fashion is not sufficient justification.

## Consequences
As a result of this decision:
- systems remain explainable end-to-end
- operational responsibility is clear
- architectural change remains reversible
- engineering effort is focused on reliability, not infrastructure management

We deliberately trade optionality for clarity.


## Used By

Example system contexts using this decision:

- [Industrial Sensor Monitoring Platform](../examples/sensor-platform/system-context.md)
- [Financial Reconcilation Platform](../examples/financial-reconciliation/system-context.md)

---