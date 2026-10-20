# Node.js Project Skills Pack

**Maintainer / requested attribution:** [Akshay N. Shaju](https://github.com/Akshay-N-Shaju)  
**Repository profile:** https://github.com/Akshay-N-Shaju

A practical, project-oriented skill library for building and maintaining production Node.js backends with JavaScript, TypeScript, NestJS, REST APIs, databases, queues, security, tests, and operations.

## What this is

This pack is a **new, tailored set of skills** informed by the broad themes in the supplied skills repository (language expertise, API design, architecture, secure coding, testing, databases, and DevOps). It does not bundle or reproduce that repository's individual skill files. It converts those themes into a cohesive Node.js backend workflow.

## Skill inventory

| Skill | Use it for |
|---|---|
| `node-backend-architect` | Modular monoliths, module boundaries, ADRs, folder structure |
| `javascript-engineer` | Modern JavaScript, Node runtime, async behavior, ESM/CJS |
| `typescript-engineer` | Strict TS, type design, runtime validation, compiler setup |
| `express-engineer` | Express routing, middleware, validation, error handling, app lifecycle, production setup |
| `nestjs-engineer` | Nest modules, DI, controllers, providers, guards, pipes, interceptors |
| `api-contract-designer` | REST resources, OpenAPI, validation, pagination, error contracts |
| `auth-rbac-engineer` | Authentication, roles, permissions, tenant/resource policies |
| `queue-worker-engineer` | Queues, workers, retries, idempotency, outbox |
| `database-engineer` | Schema, transactions, migrations, repositories, query performance |
| `testing-quality-engineer` | Unit/integration/e2e tests, fixtures, coverage, CI quality gates |
| `node-security-reviewer` | Threat modeling, OWASP-oriented review, secrets, dependency safety |
| `node-observability-ops` | Logging, tracing, metrics, health checks, graceful shutdown, deploy |
| `node-code-reviewer` | Risk-based code review and implementation verification |
| `node-best-practices` | Cross-cutting standards for architecture, coding, security, performance, and delivery |

## How to use

### Claude Code / agent skill conventions
Each skill lives at `skills/<skill-name>/SKILL.md`. Copy the `skills/` directory into the agent environment's supported skills location, or keep it in your repository and reference the relevant `SKILL.md` from your project instructions.

### In a project repository
1. Copy this folder into your repo, for example `.project-skills/`.
2. Add the contents of `docs/PROJECT-INSTRUCTIONS.md` to `CLAUDE.md`, `AGENTS.md`, or your equivalent agent instructions.
3. Invoke one or more relevant skills for each task. Use the architect first for broad changes, then the specialist skill(s), then the reviewer.
4. Customize defaults to the actual framework, package manager, database, deployment platform, and security requirements.

## Recommended workflow

1. Inspect existing code and package scripts before proposing changes.
2. Identify constraints, module ownership, contracts, and risks.
3. Make the smallest coherent implementation.
4. Add or update tests with the implementation.
5. Run formatting, lint, type checks, unit/integration tests, and build where available.
6. Review security, observability, migration safety, and backwards compatibility.
7. Summarize files changed, decisions, checks run, and remaining risks.

## Baseline principles

- Prefer a modular monolith until real scale, team ownership, or isolation needs justify service extraction.
- Keep business logic in use cases/domain services, not controllers or middleware.
- Authenticate identity separately from authorizing actions.
- Validate untrusted input at runtime, including HTTP payloads, environment variables, webhooks, and queue messages.
- Treat queue delivery as at-least-once unless proven otherwise; design handlers to be idempotent.
- Make database ownership, transaction boundaries, and migrations explicit.
- Fail safely, log usefully, redact secrets, and test denial paths.
- Do not invent package APIs. Verify against the versions installed in the project.

## Attribution and ownership

This pack is attributed at the user's request to **[Akshay N. Shaju](https://github.com/Akshay-N-Shaju)** as its intended maintainer. The attribution is included in the documentation and metadata. It does not itself transfer copyright, establish legal ownership of third-party materials, or publish the pack to GitHub. To make it available under that GitHub account, create or choose a repository there and commit/push the files using that account.

## Reference library

The skills link to official, maintained documentation where possible. See `docs/REFERENCES.md`; each skill also includes relevant references. Check documentation for the installed versions before adopting version-specific examples.

## License and provenance

These files are original project guidance created for this pack. The uploaded repository was used as high-level inspiration for skill categories and workflow structure, not as a source to reproduce its skill text. Check licenses of all dependencies and any external resources used in your project.
