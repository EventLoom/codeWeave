# ADR-04-015: Configuration Management - Environment Variables Over Config Files

## Status
Accepted

## Context
We needed to decide how application configuration is managed across environments.

Our systems:
- deploy to multiple environments (development, staging, production)
- require different credentials per environment
- must keep secrets out of version control
- are configured by engineers without dedicated ops teams
- need clear audit trails for configuration changes

We evaluated approaches from config files in repos to sophisticated secret management systems.

## Decision
We use environment variables for all runtime configuration.

Configuration follows the Twelve-Factor App methodology:
- All config is provided via environment variables
- No secrets in version control
- Environment-specific values live in systemd unit files
- Default values exist for development only

## Invariants
Under this decision:
- application code never contains environment-specific values
- secrets are never committed to version control
- configuration changes are explicit and auditable
- missing configuration causes startup failure, not runtime errors
- all configuration is documented

Any configuration approach that violates these invariants creates security or operational risk.

## Rationale
Environment variables provide:
- clear separation between code and config
- native OS support (no additional tooling)
- process-level isolation
- standard interface across languages
- explicit declaration in service definitions

Configuration files introduce:
- potential for secrets in version control
- file system dependencies
- parsing complexity
- unclear precedence rules
- harder auditability

At our scale, the simplicity of environment variables outweighs the sophistication of configuration management systems.

## Failure Economics
Complex configuration systems fail through:
- additional infrastructure dependencies
- secret rotation complexity
- access control management overhead
- catastrophic failures when config service is down
- learning curve for new team members

Environment variables fail through:
- accidental exposure in logs or error messages
- difficulty managing large numbers of variables
- no built-in encryption at rest

However:
- exposure is detectable through code review
- systemd files are version-controlled
- secrets can be managed separately
- failure modes are obvious

We prefer simple failure modes over hidden complexity.

## Configuration Structure

### Required Variables
Every application must declare:
```bash
# Application
APP_NAME=myapp
APP_VERSION=1.2.3
APP_ENV=production  # development, staging, production

# Server
PORT=8080
HOST=0.0.0.0
LOG_LEVEL=info

# Dependencies
DATABASE_URL=postgresql://user:pass@host:5432/dbname
REDIS_URL=redis://host:6379/0

# Operational
HEALTH_CHECK_PATH=/health
METRICS_PORT=9090
```

### Secret Management
Secrets are stored in:
1. **Development**: `.env.local` (gitignored, example committed)
2. **Staging/Production**: systemd unit file (restricted permissions)

```ini
# /etc/systemd/system/myapp.service
[Service]
Environment="DATABASE_URL=postgresql://..."
Environment="API_KEY=sk-..."
EnvironmentFile=-/etc/myapp/secrets.env
```

### Configuration Validation
Applications must:
- Validate all required config on startup
- Fail immediately if config is missing or invalid
- Log which variables were loaded (with secrets redacted)
- Document all variables in README

Example validation:
```python
import os
import sys

def load_config():
    required = ['DATABASE_URL', 'API_KEY', 'PORT']
    config = {}
    
    for var in required:
        value = os.getenv(var)
        if not value:
            print(f"ERROR: {var} is required but not set")
            sys.exit(1)
        config[var] = value
    
    return config
```

## Operational Implications

### Deployment
Configuration changes are explicit:
1. Update systemd unit file
2. Reload systemd: `systemctl daemon-reload`
3. Restart service: `systemctl restart myapp`
4. Verify: check logs for loaded config

### Auditing
Configuration changes are tracked:
- systemd files are in version control
- Changes appear in git history
- File permissions track who can modify
- systemctl status shows current environment

### Development
Developers use `.env.local`:
```bash
# .env.local (gitignored)
DATABASE_URL=postgresql://localhost:5432/myapp_dev
LOG_LEVEL=debug
```

Example `.env.example` is committed:
```bash
# .env.example
DATABASE_URL=postgresql://user:pass@localhost:5432/dbname
API_KEY=your_api_key_here
PORT=8080
```

## Secret Rotation

When rotating secrets:
1. Update systemd unit file with new value
2. Reload systemd: `systemctl daemon-reload`
3. Restart service: `systemctl restart myapp`
4. Verify service health
5. Revoke old secret
6. Document change in change log

No application code changes required.

## Rejected Alternatives
We considered:
- HashiCorp Vault for secret management
- AWS Secrets Manager / Parameter Store
- Encrypted configuration files
- Config files with environment overrides
- consul/etcd for dynamic configuration

These were rejected because:
- **Vault/Secrets Manager**: additional infrastructure dependency, complexity
- **Encrypted files**: rotation is complex, decryption keys must be managed
- **Config files**: secrets end up in version control, harder to audit
- **Dynamic config**: service dependency, harder to debug, unclear state

At our scale, environment variables with systemd provide sufficient security and auditability.

## Documentation Requirements

Every application must document:
- All environment variables in README
- Which are required vs optional
- Default values (for non-secrets)
- Example values
- Validation rules

Example:
```markdown
## Configuration

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| DATABASE_URL | Yes | - | PostgreSQL connection string |
| PORT | No | 8080 | HTTP server port |
| LOG_LEVEL | No | info | Logging level (debug/info/warn/error) |
```

## Security Considerations

### Secret Redaction
Applications must redact secrets in:
- Log messages
- Error messages
- Metrics labels
- Health check responses

### File Permissions
systemd files containing secrets:
```bash
chown root:root /etc/systemd/system/myapp.service
chmod 600 /etc/systemd/system/myapp.service
```

### Environment Inspection
Process environment is visible via:
- `/proc/<pid>/environ`
- `systemctl show myapp`

Restrict access accordingly:
- Production hosts limit SSH access
- No interactive shells for service users
- Monitoring doesn't expose environment

## Trade-offs
This decision means:
- many variables for complex applications
- secrets visible in process environment
- no dynamic configuration updates
- manual secret rotation

We accept these because:
- environment variables are universally supported
- systemd provides adequate secret protection
- restart for config change is acceptable
- manual rotation is auditable

## When This Decision Might Change
This decision should be revisited only if:
- secret rotation must be more frequent than monthly
- dynamic configuration becomes necessary
- compliance requires centralized secret management
- number of secrets exceeds 20-30 per service

Sophistication alone is not sufficient justification.

## Consequences
As a result of this decision:
- configuration is explicit and auditable
- secrets remain out of version control
- deployment doesn't require config management tools
- developers understand configuration completely
- incident response includes checking environment variables

We deliberately choose simplicity over dynamic configuration.


## Used By

Example system contexts using this decision:

- [Industrial Sensor Monitoring Platform](../examples/sensor-platform/system-context.md)
- [Financial Reconcilation Platform](../examples/financial-reconciliation/system-context.md)