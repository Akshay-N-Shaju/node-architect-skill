# Project instructions for coding agents

You are working on a Node.js backend. Optimize for correctness, security, clarity, and maintainability rather than cleverness.

## Before editing
- Read `package.json`, lockfile, TypeScript/compiler config, lint config, test setup, and relevant module files.
- Identify the framework (plain Node, Express, Fastify, NestJS), module system (ESM/CJS), runtime version, persistence layer, and test commands.
- Preserve established conventions unless they are unsafe or clearly inconsistent.
- Ask a focused question only when a missing requirement blocks a safe implementation. Otherwise state reasonable assumptions.

## Architecture
- Prefer business-capability modules and explicit public interfaces.
- Avoid cross-module imports from internal implementation paths.
- Keep transport, application, domain, and infrastructure concerns separate where complexity justifies it.
- Avoid speculative abstractions and premature microservices.
- Record material architectural choices as ADRs.

## Implementation
- Validate all external inputs at runtime.
- Keep controllers thin and use cases explicit.
- Make transaction boundaries clear.
- Use dependency injection where it improves lifecycle control and testability.
- Handle asynchronous errors and resource cleanup explicitly.
- Never swallow errors silently.
- Avoid breaking API contracts without a migration/deprecation plan.

## Security
- Deny by default. Check authentication, permission, tenant, ownership, and resource state as applicable.
- Never trust client-provided role/permission claims without verification.
- Never log secrets, credentials, access tokens, cookies, or sensitive payloads.
- Use parameterized queries and safe serialization.
- Add negative tests for forbidden and cross-tenant cases.

## Validation before completion
Run the checks supported by the repository, typically:
- format check
- lint
- typecheck
- unit tests
- integration/e2e tests relevant to the change
- production build

If a command cannot run, say why. Do not claim a check passed unless it was run successfully.

## Final response
Summarize:
1. What changed and why.
2. Important design choices.
3. Commands run and their outcomes.
4. Remaining risks, assumptions, or follow-up work.
