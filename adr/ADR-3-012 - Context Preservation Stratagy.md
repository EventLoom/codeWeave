# ADR-03-012: Context Preservation Strategy — Decisions Must Outlive Authors

## Status
Accepted

## Context
Systems outlive the engineers who created them.

Over time:
- original rationale is forgotten
- assumptions become implicit
- constraints are rediscovered through failure
- architectural intent decays

Code alone does not preserve decision context.

## Decision
We explicitly document architectural decisions, including rationale, trade-offs, and revisit triggers.

Documentation focuses on *why*, not just *what*.

## Invariants
- Every irreversible architectural decision must have a written record.
- Decision documents must include explicit trade-offs.
- Revisit conditions must be concrete.
- Documentation must be stored with the codebase.

## Rationale
Undocumented decisions create:
- repeated debates
- unsafe refactors
- erosion of constraints
- accidental architectural drift

Explicit decision records preserve:
- intent
- boundary conditions
- accepted risk
- operational assumptions

This reduces entropy as teams change.

## Failure Economics
Without preserved context:
- refactors reintroduce solved problems
- safety constraints are weakened
- maintenance cost increases over time
- incidents repeat prior mistakes

With preserved context:
- change impact is evaluated against known assumptions
- architectural drift is detectable
- onboarding is accelerated

## Operational Implications
- PR discussions reference decisions, not preferences.
- Deviations require explicit acknowledgement.
- Architectural change becomes deliberate.

## Rejected Alternatives
We considered:
- relying on commit history
- relying on code comments
- relying on institutional memory

These were rejected because they do not capture intent or accepted trade-offs.

## Trade-offs
This introduces:
- ongoing documentation effort
- periodic review overhead

We accept this to prevent architectural decay.

## When This Decision Might Change
Revisit only if:
- team turnover is negligible
- system lifespan is intentionally short

## Consequences
Architectural knowledge becomes durable and transferable.
