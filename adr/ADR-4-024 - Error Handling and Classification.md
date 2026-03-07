# ADR-4-024: Error Handling and Classification Model

**Status**: Accepted  
**Summary**: Defining a consistent taxonomy and representation for failures across services and APIs.  
**Key Trade-off**: Increased upfront design effort vs. predictable and diagnosable failure behaviour  
**Read this if**: You're implementing APIs, handling failures, or designing retry logic  

---

## Context

ADR-4-019 defines API stability.
ADR-5-018 defines incident restoration priorities.

However, without explicit failure semantics:

- Monitoring signals become ambiguous.
- Retry behaviour becomes inconsistent.
- Client integrations become fragile.
- Root cause analysis becomes slower.

Failure must be designed, not improvised.

---

## Decision

All failures must be:

1. Explicitly classified.
2. Logged with structured context (ADR-4-025).
3. Represented using a stable envelope.
4. Explicit about retryability.
5. Safe to expose externally.

---

## Error Taxonomy

### 1. Client Errors (4xx)

These indicate incorrect usage.

- VALIDATION_ERROR
- AUTHENTICATION_ERROR
- AUTHORIZATION_ERROR
- NOT_FOUND
- CONFLICT
- RATE_LIMITED

Default: not retryable.

---

### 2. Operational Errors (5xx)

These indicate transient or dependency failure.

- DEPENDENCY_FAILURE
- TIMEOUT
- SERVICE_UNAVAILABLE
- RESOURCE_EXHAUSTED
- UPSTREAM_UNAVAILABLE

May be retryable.

---

### 3. Internal Errors

- INTERNAL_ERROR

Represents unexpected behaviour or invariant violation.

Never expose stack traces or implementation details.

---

## Standard Error Envelope

All APIs must return:

{
  "error": {
    "code": "SERVICE_UNAVAILABLE",
    "message": "Playback service temporarily unavailable",
    "retryable": true,
    "correlation_id": "abc123"
  }
}


Retry Discipline

Only retry when explicitly allowed.

Use exponential backoff with jitter.

Non-idempotent operations must require idempotency keys.

Observability Integration

Each error must:

Emit structured logs

Increment appropriate metrics counters

Preserve correlation IDs

Consequences
Positive

Clear operational signals

Predictable integration behaviour

Safer public APIs

Negative

Requires discipline

Slight verbosity in API responses