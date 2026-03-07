
# ADR-4-025: Structured Application Logging Strategy

**Status**: Accepted  
**Summary**: Standardising structured logging across services to ensure consistent diagnostics and machine-parsable output.  
**Key Trade-off**: Slight verbosity vs. operational clarity and automation readiness  
**Read this if**: You're implementing logging or debugging production systems  

---

## Context

ADR-4-013 defines observability posture.
ADR-4-021 defines audit logging requirements.

This ADR governs operational application logs.

Without consistency:
- Correlation across services fails.
- Automated analysis becomes unreliable.
- Incident diagnosis slows.

---

## Decision

All services must emit structured logs.

---

## Format Requirements

Logs must:

- Be machine-parseable (JSON or structured key-value).
- Emit to stdout.
- Use UTC timestamps.
- Contain consistent core fields.

---

## Required Fields

- timestamp
- level
- service
- message
- correlation_id (if applicable)

Optional fields:
- request_id
- operation
- duration_ms
- error_code
- resource_id

---

## Log Levels

- DEBUG (development only)
- INFO (default production)
- WARN
- ERROR

---

## Redaction Rules

Never log:

- Secrets
- Credentials
- Tokens
- Full personal data
- Raw request payloads containing sensitive fields

---

## Separation of Concerns

- Operational logs → this ADR
- Security audit logs → ADR-4-021
- Metrics → ADR-4-013

---

## Consequences

### Positive

- Improved cross-service diagnosis
- Compatible with log aggregation
- Enables structured alerting

### Negative

- Schema enforcement required
- Slight increase in payload size


## Used By

Example system contexts using this decision:

- [Industrial Sensor Monitoring Platform](../examples/sensor-platform/system-context.md)
- [Financial Reconcilation Platform](../examples/financial-reconciliation/system-context.md)