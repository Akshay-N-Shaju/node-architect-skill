# Node.js backend PR checklist

## Correctness
- [ ] Requirements and edge cases are covered.
- [ ] Async errors and resource cleanup are handled.
- [ ] Business invariants are enforced in the appropriate layer.
- [ ] API compatibility is preserved or migration is documented.

## Security
- [ ] Inputs are runtime-validated.
- [ ] Authentication and authorization are explicit.
- [ ] Tenant/resource ownership is checked.
- [ ] Secrets and sensitive data are not logged.
- [ ] Abuse controls and request limits are appropriate.

## Data and jobs
- [ ] Migration is safe for overlapping app versions.
- [ ] Transactions and concurrency behavior are understood.
- [ ] Job handlers are idempotent and retries are bounded.
- [ ] Outbox or recovery behavior exists where required.

## Quality and operations
- [ ] Tests cover success, failure, and denial paths.
- [ ] Lint, typecheck, tests, and build were run.
- [ ] Logs/metrics are useful and redacted.
- [ ] Startup/shutdown and deployment implications are considered.
- [ ] Docs/config examples are updated.
