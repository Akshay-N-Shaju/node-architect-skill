# Suggested task workflow

## New feature
1. `node-backend-architect`: map the capability, ownership, and module API.
2. `api-contract-designer`: define request/response and error contracts if exposed over HTTP.
3. Choose implementation specialist: `nestjs-engineer`, `javascript-engineer`, or `typescript-engineer`.
4. Add `auth-rbac-engineer` for protected actions and `database-engineer` for persistence.
5. Add `queue-worker-engineer` for asynchronous work.
6. `testing-quality-engineer`: design tests and failure cases.
7. `node-security-reviewer` and `node-code-reviewer`: final risk-based review.
8. `node-observability-ops`: check runtime visibility and deployment impact.

## Bug fix
1. Reproduce and identify the root cause.
2. Add a regression test that fails before the fix where practical.
3. Make the smallest safe correction.
4. Run the relevant tests plus lint/typecheck/build.
5. Review adjacent failure paths and logging.

## Refactor
1. Capture current behavior with tests.
2. State invariants and compatibility constraints.
3. Refactor in small steps without mixing unrelated feature work.
4. Compare behavior and performance if relevant.
5. Document material changes in an ADR.

## Production incident
1. Establish impact, timeline, and evidence.
2. Prefer mitigation that is reversible and low-risk.
3. Preserve useful logs/metrics/traces while protecting sensitive data.
4. Avoid speculative production mutations.
5. Document root cause, contributing factors, remediation, and prevention.
