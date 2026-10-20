---
name: node-best-practices
description: Applies cross-cutting Node.js backend best practices for maintainable architecture, reliable async code, secure configuration, API contracts, database safety, queue semantics, testing, observability, and delivery. Use during implementation planning or final review.
---

# Node.js Best Practices

## Default decision rules
- Prefer a modular monolith before microservices unless scale, team ownership, compliance, or isolation requirements justify distribution.
- Prefer the simplest implementation that preserves domain rules and operational safety.
- Inspect installed versions and current project conventions; verify version-specific APIs in official documentation.
- Keep business logic framework-independent where practical.
- Do not add dependencies without explaining the capability gap and maintenance cost.
- Don't claim tests, security controls, or deployment checks passed unless verified.

## JavaScript and TypeScript
- Use a supported Node.js LTS release and pin toolchain expectations.
- Pick ESM or CommonJS deliberately and keep runtime, tests, and build config consistent.
- Use strict TypeScript when TypeScript is selected.
- Validate external values at runtime; types are erased at runtime.
- Avoid floating promises, unbounded concurrency, blocking CPU work on the event loop, and silent catch blocks.
- Apply timeouts, cancellation, backpressure, and cleanup to long-running or external operations where appropriate.

## Architecture
- Organize code by business capability.
- Define module ownership and public interfaces.
- Keep controllers and middleware thin; use cases orchestrate business operations.
- Keep transaction boundaries explicit.
- Avoid circular dependencies and generic abstraction layers without a concrete need.
- Record consequential decisions in ADRs.

## Express and HTTP
- Configure security middleware, body limits, explicit CORS, rate limiting, request IDs, validation, not-found handling, and centralized errors.
- Set `trust proxy` only for the real ingress topology.
- Keep API errors stable and non-sensitive.
- Make authentication and authorization explicit per route/use case.
- Add timeouts and limits at application and infrastructure layers as appropriate.

## Security
- Deny by default and test authorization denial paths.
- Validate every trust boundary: HTTP, environment, database JSON, files, webhooks, queues, and external APIs.
- Use parameterized SQL and safe filesystem/path handling.
- Prevent SSRF and unsafe redirects where user input influences outbound URLs.
- Use established identity, session, password-hashing, and cryptographic libraries.
- Redact secrets from logs, telemetry, error responses, and job payloads.
- Audit dependencies and have a patch-response process.

## Persistence
- Enforce critical invariants with database constraints where practical.
- Avoid N+1 queries and unbounded result sets.
- Measure query plans before adding indexes or caching.
- Use migrations and compatibility-aware rollout plans.
- Keep transactions short and avoid external network calls inside transactions.
- Design concurrency handling for conflicting writes.

## Queues and events
- Assume at-least-once delivery unless documented otherwise.
- Make handlers idempotent, payloads versioned, retries bounded, and failures observable.
- Use transactional outbox for critical DB-to-queue consistency.
- Protect dead-letter replay and administrative queue operations.
- Keep queue concurrency within database and provider capacity.

## Quality
- Add tests for success, boundaries, failure modes, concurrency, and authorization denial.
- Run format, lint, typecheck, tests, and build as applicable.
- Use integration tests to verify actual database/queue behavior when mocks would hide risk.
- Review migration safety, API compatibility, logging redaction, and shutdown behavior.

## Production readiness
- Emit structured logs, low-cardinality metrics, and traces where useful.
- Separate liveness from readiness.
- Handle SIGTERM gracefully.
- Run migrations as a controlled release step.
- Verify backups by testing restores.
- Document rollout, rollback, and incident response.

## References
- Node.js best practices and security: https://nodejs.org/en/learn
- Node.js security best practices: https://nodejs.org/en/learn/getting-started/security-best-practices
- Express security: https://expressjs.com/en/advanced/best-practice-security/
- Express performance/reliability: https://expressjs.com/en/advanced/best-practice-performance/
- OWASP Cheat Sheet Series: https://cheatsheetseries.owasp.org/
- OWASP API Security: https://owasp.org/www-project-api-security/
- TypeScript handbook: https://www.typescriptlang.org/docs/handbook/intro.html
- NestJS documentation: https://docs.nestjs.com/
